Angular2 动态的创建组件并插入到Shadow Dom中
--

> 作者随时修改，为方便读者追本朔源，转载请保留地址。


**前言：**

- 为什么会有这个需求？

	因为在开发组件中，难免会有一些组件是需要动态生成的，以减少Document中Dom 数量，节省内存开支。
	例如全局的 message 组件、Alert 组件、Notice 组件等。
	
- angular2 中如何动态的编译Template？

	在 Ng2 中， 废除了 $compiled 这个方法，用户将不能直接编译模板，如果想动态的创建组件，必须借助	组件工厂 (componentFactoryResolver)
	
	
**如何动态创建一个组件：**

这里以 Tooltip 组件为例

1. 首先需要准备一个组件内容(用来插入到页面中的动态组件)  
	这个组件就是很常规的组件， 没有什么特别的东西,只需要准备一些变量同步Template即可。
	
	```
	import { Component } from '@angular/core';

	@Component({
    	selector: 'pxx-tooltip-container',
	    templateUrl: './tooltip.html',
    	styleUrls: ['./tooltip.scss'],
	})
	export class TooltipContainer {
	    _top: any;
    	_left: any;
	    _state = '';
    	message: string;
	    placement: 'top' | 'bottom' | 'left' | 'right' = 'top';

    	_show: boolean = false;
	}
	```

2. 将这个内容组件添加到Module 中，这一步主要是将这个组件存入 Angular 的工厂缓存中。  
	App.module （或者是其他的Module，甚至是这个组件自身的Module 都可以，只要最终在AppModule 	中 import）, 我这里是组件自身的Module，最终导入到App.Module中。  
	
		// TooltipModule
		
		NgModule({
    		imports: [
        		TooltipModule
    		],
		    declarations: [
        		TooltipContainer
			],
			exports: [
    			TooltipContainer
		    ],
		    entryComponents: [
    		    TooltipContainer // 这里很重要，将这个组件放入Angular 工厂缓存中
    		]
		})
		export class SharedModule {
		    static share(): ModuleWithProviders {
        		return {
		            ngModule: SharedModule,
        		    providers: []
				};
	    	}

		}
	
	
3. 建立一个中转组件、或者 service 来操作生成的组件， 这里我创建了一个 Tooltip.component.ts 的	组件来做中转，也是用户真实使用的组件

		import {
            Component, Input, HostListener, ComponentRef, ComponentFactoryResolver, ApplicationRef, ViewContainerRef,
            ReflectiveInjector
        } from '@angular/core';
        import { TooltipContainer } from './tooltip.container';

        @Component({
            selector: 'pxx-tooltip',
            template: '<ng-content></ng-content>',
        })
        export class TooltipComponent {
            container: ComponentRef<any>;

            @Input() message: string;
            @Input() placement: 'top' | 'bottom' = 'top';

            private _top: any;
            private _left: any;

            constructor(private componentFactoryResolver: ComponentFactoryResolver,
                        private appRef: ApplicationRef) {
            }

			// 给中转组件绑定鼠标移入事件
            @HostListener('mouseenter', ['$event.target'])
            enter(el) {
            
            	// 当CreateTips 执行完毕后计算位置
                this._createTips().then( () => {
                    this._getOffset(el);
                });
                
                //添加一个进入动画， 200ms 后移除
                this._createTimeout('enter', 200);
                
                // 通滚 instance 控制动态组件的属性，显示组件。
                this.container.instance._show = true;
            }

            @HostListener('mouseleave', ['$event.target'])
            leave() {
                // 留 80 ms 给动画效果, 然后摧毁组件
                this._createTimeout('leave', 80, () => {
                    this.container.destroy();
                    this.container = null;
                });
            }

            private _createTips<T>(): Promise<T> {
                return new Promise((resolve, reject) => {
                    if (!this.container) {
                        if (!this.appRef['_rootComponents'].length) {
                            const err = new Error('AppRoot 未找到.');
                            console.error(err);
                            reject(err);
                        }

                        // 查找Body 的位置
                        let appContainer: ViewContainerRef = this.appRef['_rootComponents'][0]['_hostElement'].vcRef;

                        let providers = ReflectiveInjector.resolve([
                            // 此处创建提供商, 但是这个组件不需要 provider
                        ]);

                        // 创建并加载ToolTipContainer 组件，这里从Factory 中讲组件提取出来，创建组件。
                        let tooltipFactory = this.componentFactoryResolver.resolveComponentFactory(TooltipContainer);
                        let childInjector = ReflectiveInjector.fromResolvedProviders(providers, appContainer.parentInjector);
                        this.container = appContainer.createComponent(tooltipFactory, appContainer.length, childInjector);

                        this.container.instance.placement = this.placement;
                        this.container.instance.message = this.message;

                        // resolve 加入异步队列， 模拟一个 nextTick 的操作，当Angular 执行完下一个时间循环后调用。
                        setTimeout(() => resolve(), 0);
                    }

                    // resolve();
                });
            }

			/**
			 * 这里是计算 tooltipContaine 的显示坐标的
			 */
            private _getOffset(el) {
                let tooltip = <HTMLElement>document.querySelector('#tooltip');

                if (this.placement == 'top') {

                    this._left = (el.getBoundingClientRect().left + (el.offsetWidth - tooltip.offsetWidth) / 2) + 'px';
                    this._top = el.getBoundingClientRect().top - el.offsetHeight - tooltip.offsetHeight + (el.offsetHeight / 						1.28) + 'px';

                } else if (this.placement == 'bottom') {
                    this._left = (el.getBoundingClientRect().left + (el.offsetWidth - tooltip.offsetWidth) / 2) + 'px';
                    this._top = el.getBoundingClientRect().top + el.offsetHeight + 'px';
                }

                // instance
                this.container.instance._left = this._left;
                this.container.instance._top = this._top;
            }

            private _createTimeout(state, delay, cb?: Function) {
                this.container.instance._state = state;
                setTimeout(() => {
                    cb && cb();
                }, delay);
            }
        }

4. 将 TooltipComponent 这个中转组件也添加进APPModule
5. 使用：

	<pxx-tooltip message="这是一个优雅的提示">Hover </pxx-tooltip>


	
** 关于方法二 **

这篇文章被我命名为 *方法一*， 所以还会有方法二。  
相对于上文这种方法， 方法二功能更强大，然而更死板。  
方法二的使用的核心API是 `compileModuleAndAllComponentsAsync` `RuntimeCompiler`.  
方法二相对于方法一：

- 好处就是不用每次把需要动态加载的组件放到 entryComponent 中缓存。但是需要动态的创建 ComponentFactory，完全的构造一个新的Module，这个Module 自然就包含了动态创建的组件。
- 坏处就是创建的Module 完全独立，无论作用域、Module通信等。也就意味着创建出来的Module无法使用外部Module 已经 import 过的模块、指令、服务、组件等，这真让人感到悲伤。。

那么方法二有什么用呢？ 

个人觉得应用场景是用与解决动态的切换组件