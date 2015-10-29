## angularjs 

### 判断两个对象是否相等的隐藏方法

	angular.equals()


### ng-click 触发两次的问题
	<label ng-class="{selected: key == selectObj.tp}" ng-click="switchTp(value);">
        <input type="radio" class="hidden" ng-model="selectObj.tp" ng-click="sendEvent(selectObj.tp)" ng-value="key"/>
        <span ng-bind="value"></span>
    </label>

	此处绑定 ng-click 在 label 上的时候， 会触发两次 switchTP 事件。 
	而解决方法就是拆分 ng-click到子级的 input 上。 
	触发两次 click 的原因暂未知。 
	目前猜测是由于 ng-click , ng-model 系统默认触发 $digest() 的问题
