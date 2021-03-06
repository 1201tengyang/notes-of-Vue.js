# 1. Vue.js是什么?
	1). 一位华裔前Google工程师(尤雨溪)开发的前端js库
	2). 作用: 动态构建用户界面
	3). 特点:
		* 遵循MVVM模式
		* 编码简洁, 体积下, 运行效率高, PC/移动端开发都合适
		* 它本身只关注UI, 可以轻松引入vue插件和其它第三库开发项目
	4). 与其它框架的关联:
		* 借鉴angular的模板和数据绑定技术
		* 借鉴react的组件化和虚拟DOM技术
	5). vue包含一系列的扩展插件(库):
		* vue-cli: vue脚手架
		* vue-resource(axios): ajax请求
		* vue-router: 路由
		* vuex: 状态管理
		* vue-lazyload: 图片懒加载
		* mint-ui: 基于vue的组件库(移动端)
		* element-ui: 基于vue的组件库(PC端)
  
# 2. 基本使用
	1). 引入vue.js
	2). 创建Vue对象(vm), 指定选项(配置)对象
		el : 指定dom标签容器的选择器
		data : 指定初始化状态属性数据的对象函数(返回一个对象)
	3). 在页面模板中使用{{}}和vue指令
		
# 3. Vue对象的选项
## 1). el
	指定dom标签容器的选择器
	Vue就会管理对应的标签及其子标签
## 2). data
	对象或函数类型
	指定初始化状态属性数据的对象
	vue对象可以直接访问其属性
	页面中可以直接访问使用
	数据代理: 由vm对象来代理对data中所有属性的操作(读/写)
## 3). methods
	包含多个方法的对象
	供页面中的事件指令来绑定回调
	回调函数默认有event参数, 但也可以指定自己的参数
	所有的方法由vue对象来调用, 访问data中的属性直接使用this.xxx
## 4). computed
	包含多个方法的对象
	对状态属性进行计算返回一个新的数据, 供页面获取显示
	一般情况下是相当于是一个只读的属性
	利用set/get方法来实现属性数据的计算读取, 同时监视属性数据的变化
	如何给对象定义get/set属性
		在创建对象时指定: get name () {return xxx} / set name (value) {}
	  	对象创建之后指定: Object.defineProperty(obj, age, {get(){}, set(value){}})
	计算属性高级:
	  通过get/set方法实现对属性数据的显示和监视
	  计算属性存在缓存, 多次读取只执行一次计算
## 5). watch
	Vue.$watch()
	包含多个属性监视的对象
	分为一般监视和深度监视
		'xxx' : {
			deep : true,
			handler : fun(value)
		}

# 4. 过渡动画
	利用vue去操控css的transition/animation动画
	模板: 使用<transition name='xxx'>包含带动画的标签
	css样式
		.fade-enter-active: 进入过程, 指定进入的transition
		.fade-leave-active: 离开过程, 指定离开的transition
		.xxx-enter, .xxx-leave-to: 指定隐藏的样式
	编码例子
	    .xxx-enter-active, .xxx-leave-active {
	      transition: opacity .5s
	    }
	    .xxx-enter, .xxx-leave-to {
	      opacity: 0
	    }
	    
	    <transition name="xxx">
	      <p v-if="show">hello</p>
	    </transition>
    
# 5. 生命周期
	vm/组件对象
	生命周期图
	主要的生命周期函数(钩子)
    	created(): 启动异步任务(启动定时器,发送ajax请求, 绑定监听)
    	beforeDestroy(): 做一些收尾的工作

	<!--
	1. vue对象的生命周期
	  1). 初始化显示
	    * beforeCreate()
	     * 在此之间实现数据代理 
	    * created()
	     * 在此之间实现编译模板 （将模板中的JS语法替换成具体的值）
	    * beforeMount()
	    * mounted()
	  2). 更新状态
	    * beforeUpdate()
	    * updated()
	  3). 销毁vue实例: vm.$destory()
	    * beforeDestory()
	    * destoryed()
	2. 常用的生命周期方法
	  created()/mounted(): 发送ajax请求, 启动定时器等异步任务
	  beforeDestory(): 做收尾工作, 如: 清除定时器
	-->

# 6. 页面指令
	v-text/v-html: 指定标签体
    	* v-text : 当作纯文本
		* v-html : 将value作为html标签来解析
	v-if v-else v-show
		* v-if : 如果vlaue为true, 当前标签会输出在页面中（通过移除，添加标签来实现）
		* v-else : 与v-if一起使用, 如果value为false, 将当前标签输出到页面中，不需要表达式
		* v-show: 就会在标签中添加display样式, 如果vlaue为true, display=block, 否则是none  
		* 比较v-if与v-show
  		--如果需要频繁切换 v-show 较好，如果在运行时条件不大可能改变 v-if 较好
	v-for : 遍历
		* 遍历数组 : v-for="(person, index) in persons"   
		* 遍历对象 : v-for="value in person"   $key
		* 遍历得到的每一项建议都添加一个唯一标识key (:key:'index')
	v-on : 绑定事件监听
		* v-on:事件名, 可以缩写为: @事件名
		* 监视具体的按键: @keyup.keyCode   @keyup.enter
		* 停止事件的冒泡和阻止事件默认行为: @click.stop   @click.prevent
		* 隐含对象: $event
	v-bind : 强制绑定解析表达式  
		* 很多属性值是不支持表达式的, 就可以使用v-bind
		* 可以缩写为:  :id='name'
		* :class
		  * :class="a"
			* :class="{classA : isA, classB : isB}"
			* :class="[classA, classB]"
		* :style
			:style="{color : color}"
	v-model
		* 双向数据绑定
		* 自动收集用户输入数据
	ref : 标识某个标签
		* ref='xxx'
		* 读取得到标签对象: this.$refs.xxx
  
# 7. 自定义指令
	1). 注册全局指令
	    Vue.directive('my-directive', function(el, binding){
	      el.innerHTML = binding.value.toUpperCase()
	    })
	2). 注册局部指令
	    directives : {
	      'my-directive' : function(el, binding) {
	          el.innerHTML = binding.value.toUpperCase()
	      }
	    }
	3). 使用指令
	    <div v-my-directive='xxx'>
# 8. 注意  
    1）vue中的data里的数组的数组方法并不是原生的，而是经过包装的，使用时其实做了两件事（
		1.调用原生的数组方法，
		2.更新页面 
	）
	push()
	pop()
	shift()
	unshift()
	splice()增加，删除，替换
	sort()
	reverse()

	不能通过下标来改变数组内部的数据，vue不会知道变化，可以通过数组的方法来实现  

    2)页面的显示是根据数据来显示的，要想改变初始化页面，就要改变初始化数据   
    3）input的两种DOM事件监听
      -input事件。输入过程中触发
      -change事件，失去焦点时触发（v-model.lazy就是change监听） 
    4）vm代理data对象中的数据