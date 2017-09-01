Ng2 指令无法获取到参数
--

> 背景：创建指令后，@Input() 过来的参数是 undefined


代码如下：

	import {Directive, Renderer, ElementRef, Input} from '@angular/core';

	@Directive({
    	selector: '[auto-height]'
	})
	
	export class AutoHeightDirective {

    	@Input('auto-height') pad ?: any;
    	constructor (private _render: Renderer, private el: ElementRef) {
    		console.log(this.pad) //undefined
    	};
    	
    }
    
    
原因： constructor 执行比 @Input 早，所以这个时候还没有初始化 Input 方法。

解决办法：

	export class AutoHeightDirective {

    	@Input('auto-height') pad ?: any;
    	constructor (private _render: Renderer, private el: ElementRef) {
    		
    	};
    	
    	ngOnInit () {
    		console.log(this.pad) //undefined
    	}
    	
    }
    
将逻辑初始化代码放到 ngOnInit 中即可。 ngOnInit 方法是在Input 后执行的