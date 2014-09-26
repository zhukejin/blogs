原生javascript的继承封装
--

定义参数及 `__extends` 继承函数：

	  var __hasProp = {}.hasOwnProperty,
	  __extends = function(child, parent) { 
	        for (var key in parent) {
	            if (__hasProp.call(parent, key)) 
	                child[key] = parent[key]; 
	        } 
	        function ctor() { 
	            this.constructor = child; 
	        } 
	        ctor.prototype = parent.prototype; 
	        child.prototype = new ctor(); 
	        child.__super__ = parent.prototype; 
	        return child; 
	    };
	
### 使用方法示例

定义父类：
	
	var child, parent;
	parent = (function() {
	  function parent(name) {
	    this.name = name;
	    console.log(this.name);
	  }
	
	  parent.prototype.gods = function() {
	    return console.log("" + this.name + " is a god");
	  };
	
	  return parent;
	
	})();

定义子类：
	
	child = (function(_super) {
	  __extends(child, _super);
	
	  function child() {
	    return child.__super__.constructor.apply(this, arguments);
	  }
	
	  child.prototype.kill = function(killer) {
	    return console.log("" + killer + " killed " + this.name);
	  };
	
	  return child;
	
	})(parent);

生成对象：
	
	var test;
	test = new child("耶稣"); //耶稣
	test.gods(); //耶稣 is a god
	test.kill("犹大"); //犹大 killed 耶稣


食之无味，弃之可惜。
