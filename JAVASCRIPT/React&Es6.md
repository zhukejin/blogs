Es6 & React 笔记
--

### 一. 基本的组件

如果使用 WebPack 来开发程序， 通过 Babel 编译es6，将会享受到 Es6 语法的快感..((。・_・)/~~~)

React By Es5 创建组件：

	var Test = React.createClass({
		render: function() {
			return <span>test</span>
		}
	});


By Es6：

	class Test extends React.Component {
		render() {
			return <span>test</span>
		}
	}

###### tip： 每一个组件都必须提供一个 render 方法， 以供Render.Dom 使用，render 方法返回一串Dom ， 这串Dom 只能包含一个顶级元素（`<span>`），如果出现 `return <span></span><p></p>` 这样的并列元素，就会报错。

### 二： 创建一个有State 的组件

State 是React 组件可变部分的的属性，在Es5 方法中，使用 `getInitialState` 方法来初始化state， Ex：

	var Test = React.createClass({
	    getInitialState: function () {
	        return {
	            disabled: true
	        }
	    },
		click: function() {
			this.setState({
	            disabled: !this.state.disabled
	        });
		},
	    render: function() {
	        return <div>
	          <Switch checkedChildren="开" unCheckedChildren="关" disabled={this.state.disabled}/>
	          <span> </span>
	          <Button  onClick={this.click} type="primary">LZSB</Button>
	        </div>
	    }
	});

在上面代码中， getInitialState 方法返回一个对象，对象就是 state 的集合, disabled 在组件中调用就是 this.state.disabled, 这里 disabled 是不可写的(**immutable**), 只能通过 **this.setState()** 方法来修改。 上述代码实现的就是通过 点击button 来切换 Switch 组件是否 disabled。

用Es6 来实现一下：

	class Test extends React.Component {
	    getInitialState () {
	            return {
	                disabled: true
	            }
	        }
	    click() {
	        this.setState({
	            disabled: !this.state.disabled
	        });
	    }
	    render(){
	        return <div>
	          <Switch checkedChildren="开" unCheckedChildren="关" disabled={this.state.disabled}/>
	          <span> </span>
	          <Button  onClick={this.click} type="primary">LZSB</Button>
	        </div>
	    }
	}

会发现报错： <font color="red">Warning: getInitialState was defined on Test, a plain JavaScript class. This is only supported for classes created using React.createClass. Did you mean to define a state property instead?</font>....

查询一番资料发现

	The API (via 'extends React.Component') is similar to React.createClass with the exception of getInitialState. 
	Instead of providing a separate getInitialState method, you set up your own state property in the constructor.

就是在新的API（for Es6） 中，抛弃了`getInitialState` 这个 `hook` 函数， 而是通过 constructor 构造函数来初始化 state

	constructor(props) {
        super(props);
        this.state = {
            disabled: true
        };
    }

那么在我把 `getInitialState` 方法替换成构造函数后，页面不报错了，但是点击button 切换状态的时候..: 

	Uncaught TypeError: Cannot read property 'setState' of null

为啥？ 在click 函数中 console.log(this) ，结果为 null。

在 React.createClass 中，getInitialState 做了一些其他的工作来保证 this 指向组件本身，当我们使用 constructor 的时候，this 其实就不指向组件本身了，知道这个就好说了，我们来添加一句：

	constructor(props) {
        super(props);
        this.state = {
            disabled: true
        };
    }

	this.click = this.click.bind(this); //ok, 强行指向组件本身。

<del>我的天, 这东西怎么这么麻烦！

其实还有一个终极解决方案---- **Arrow！**

### 三. Arrow

[Es6 的特性  arrow functions (点我点我，我是链接) ](https://babeljs.io/docs/learn-es2015/#arrows)，如果用过 CoffeeScript 的应该很熟悉， 摘抄一句话：

>ES6 的 arrow 函数体分享相同的词  this，用这来围绕他们的代码，这些可以达到我们预期的结果，也是 ES7 属性初始化程序在域内的方式。

	class Test extends React.Component {
	    constructor(props) {
	        super(props);
	        this.state = {
	            disabled: true
	        };
	        // this.click = this.click.bind(this)
	    }
	    click () {
	        this.setState({
	            disabled: !this.state.disabled
	        });
	    }
	    render(){
	        return <div>
	          <Switch checkedChildren="开" unCheckedChildren="关" disabled={this.state.disabled}/>
	          <span> </span>
	          <Button  onClick={this.click} type="primary">LZSB</Button>
	        </div>
	    }
	}

如此就解决了上述问题。