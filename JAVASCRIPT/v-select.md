配合Select2 使用的下拉框指令
--

>原生的下拉框实在太丑，如果项目中使用了 select2、jquery 的话， 可以使用这个指令。  
>这个指令编写是为了解决我们当前项目中下拉框、级联下拉框的问题。

### 官网select2 指令：

	Vue.directive('select', {
	  twoWay: true,
	  priority: 1000,
	
	  params: ['options'],
	    
	  bind: function () {
	    var self = this
	    $(this.el)
	      .select2({
	        data: this.params.options
	      })
	      .on('change', function () {
	        self.set(this.value)
	      })
	  },
	  update: function (value) {
	    $(this.el).val(value).trigger('change')
	  },
	  unbind: function () {
	    $(this.el).off().select2('destroy')
	  }
	})

上述指令可以解决单个输入框的绑定，功能很简陋，现在扩展一下以达到更丰富、定制化的功能。


### 指令代码：

	Vue.directive('select', {
	twoWay: true, //代表值是双向绑定的
	priority: 1000, //优先等级

	/*
	 * option： 下拉框元素内容，遵循select规则。
	 * placeholder： 默认显示的文字
	 * index： 当前索引（当有很多个select的情况）
	 * width： 宽度
	 */
	params: ['options', 'placeholder', 'index', 'width'],

	bind: function () {
		//console.log(this.params.options)
		var self = this
		$(this.el).select2({
			width: this.params.width || '120px',
			minimumResultsForSearch: -1,
			data: this.params.options,
			placeholder: this.params.placeholder
		}).on('change', function () {
			self.set(this.value) //在触发select2 的change 事件后，同步更新 vue 中的数据。

			//进行初始化判断，在进行级联、ajax 加载的时候使用，以便于改变第二个级联select 的option 数据。

			if (this.value && $(self.el).data('area') == 'province' && (backList.length == 0 || backList[self.params.index].cityList.length == 0 || address) ) {
				$.get('getCityList.htm?provinceId=' + this.value, function (data) {
					if (data) {
						var res = $.parseJSON(data);
						if (res.cityList) {
							var cityArray = $.parseJSON(res.cityList);
							var cityList = cityArray.map(function (v) {
								return {
									id: v.id,
									text: v.shortName
								}
							});
						} else {
							var cityArray = [], cityList = []
						}

						address.address[self.params.index].cityList = cityList;
					}
				})
			}
		});

		//通过 setTimeout 异步队列在初始化select 框后，删除下拉箭头（业务需要）
		setTimeout(function () { $('.select2-selection__arrow').remove() }, 0)
	},
	
	//监控参数变化，因为select2 会把前一次的参数变成真实的option元素append 到当前select 元素上，这里通过option参数监控
	paramWatchers: {
		options: function (val, oldVal) {
			//清除option
			var defaultOpt = $(this.el).find('option')[0];
			$(this.el).html($(defaultOpt));
			this.bind();
		}
	},
	update: function (value) {
		$(this.el).val(value).trigger('change')
	},

	unbind: function () {
		$(this.el).off().select2('destroy')
	}
})


### Vue实例代码：

	
	var backList = []; //存储备份的 CityList
	
	var address = new Vue({
		el: '#address',
		data: {
			address: [{cityList: [], province: '', city: '', detail: ''}],
			provinceList: [],
		},
		methods: { //删除、添加地址列表
			addAddress: function () {
				if (this.address.length == 10) {
					return;
				}
				this.address.push({cityList: [], province: '', city: '11', detail: ''});
			},
			del : function (index) {
				this.address.splice(index, 1);
			}
		},
		//在完成编译前进行数据初始化
		beforeCompile: function () {
			var plist = $.parseJSON($('#provinceList').val());
			this.provinceList = plist.map(function (v) {
				return {
					id: v.id,
					text: v.shortName,
				}
			});
	
			//初始化的时候需要后端提供当前选中的省的城市列表，以便解决初始化指令未编译完成的时候，Vue对城市进行赋值，然后引起触发change方法=> 和set() 导致 city 值重新变为空值的问题。

			if ($('#addressCommon').val()) {
				var commonAddress = $.parseJSON($('#addressCommon').val());
				this.address = backList = commonAddress.map(function (v) {
					return {
						cityList: (function () {
							return v.cityList.map(function (vv) {
								return {
									id: vv.id,
									text: vv.shortName
								}
							})
						})() || [],
						province: v.provinceId,
						city: v.cityId,
						detail: v.address,
					}
				});
			}
		}
	});


### HTML

	<div class='fill-form-1-label'>活动地点<span class='require-star'>*</span></div>
	<div v-for="item in address" :class="{'address-margin' : $index != 0}">
		<select data-area="province" class="province" val=""  v-select="item.province" :options="provinceList" :index='$index' placeholder="请选择省">
			<option value="">请选择省</option>
		</select>

		<select  class="city" v-select="item.city" val=""  :init='"cityInit" + ($index + 1)' :index='$index' :options="item.cityList" placeholder="请选择城市">
			<option value="">请选择城市</option>
		</select>
		<div class='add-address-btn' @click="addAddress">
			＋添加地址
		</div>