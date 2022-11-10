## VUE

### Vue基础

#### vue是什么

一套用于 **构建用户界面** 的 **渐进式（自底向上逐层应用，简单应用只使用轻量级的核心库，复杂应用引入各种插件库）** JavaScript框架

#### 1.1 Vue特点

1. 遵循MVVM模式
2. 编码简洁，体积小，运行效率高，适合移动/PC端开发
3. 本身只关注UI，可引入第三方库开发

#### 	Vue周边库

+ vue-cli ：vue脚手架
+ vue-resource
+ axios
+ vue-router：路由
+ vuex：状态管理
+ element-ui：基于vue的UI组件库

#### 1.2 初识Vue

+ 若要Vue工作则必须创建Vue实例，同时要传入**配置对象**
+ root容器中的代码当中混入特殊的Vue语法
+ root容器中的代码被称为**Vue模板**
+ Vue实例与容器是一一对应的
+ **{{xxx}}**中的内容要写**JS表达式**，且可以自动读取到data中的所有属性
  + js表达式：一个表达式会生成一个值，可以放在任何一个需要值的地方 
    + a   、  a+b   、   三元表达式

  + js代码：if语句、for循环语句

+ data中数据发生变化，模板中用到该数据的地方也会自动更新

```html
<body>
    <!-- 准备好一个容器 -->
    <div id="root">
        <h1>Hello！{{name}}!</h1>
    </div>

    <script>
        Vue.config.productionTip = false 
        // 阻止vue在启动时生成生产提示
        new Vue({
            el:'#root', 
        //el用于指定当前Vue实例为哪个容器服务，值通常为css选择器字符串
            data:{ 
                //data用于存储数据，数据共el所指定的容器去使用
                name:'JOJO',
                address:'我这是在哪儿'
            }
        })
    </script>
</body>
```

#### 1.3 模板语法

1. 插值语法：
   + **用于解析标签体内容**
   + 写法：**{{js表达式}}**，
2. 指令语法：
   + **解析标签**（标签属性、标签体内容、绑定事件...）
   +  `<a v-bind:href="xxx">`或简写为`<a :href="xxx">`,直接读取到data中的所有区域

```html
<body>
    <div id="root">
        <h1>插值语法</h1>
        <h3>你好，{{name}}!</h3>
        <hr>
        <h1>指令语法</h1>
        <a v-bind:href="url">快去看新番！</a><br>
        <a :href="school.url2">快去百度！</a>
    </div>

    <script>
        Vue.config.productionTip = false 
        new Vue({
            el:'#root', 
            data:{ 
                name:'JOJO',
                url:'https://www.bilibili.com/',
                school: {
                    url2:'http://www.baidu.com'
                }
            }
        })
    </script>
</body>
```

#### 1.4 数据绑定

1. 单向绑定（**v-bind**）：数据只能从data流向页面

2. 双向绑定（**v-model**）：数据不仅能从data流向页面，还能从页面流向data，

   **v-model默认收集的就是value值**

3. 双向绑定只能应用在**表单类元素**，例如` <input> <select> <textarea>`

```html
<body>
    <div id="root">
        单向数据绑定：<input type="text" :value="name"><br>
        双向数据绑定：<input type="text" v-model:value="name">
        双向数据绑定：<input type="text" v-model:"name">
    </div>

    <script>
        Vue.config.productionTip = false 
        new Vue({
            el:'#root', 
            data:{
                name:'JOJO'
            }
        })
    </script>
</body>
```

#### 1.5 el与data的两种写法

1. el的两种写法：

+ new Vue时候配置el属性
+ 先创建Vue实例，之后通过vm.$mount('#root') 指定el的值

2. data的两种写法：

+ 对象式
+ 函数式，组件开发时必须使用函数

> 由 Vue 管理的函数，一定不要写箭头函数，否则this的指向就不再是Vue实例

```html
<body>
    <div id="root">
        <h1>Hello,{{name}}!</h1>
    </div>
    <script>
        Vue.config.productionTip = false 
        //el(element)的两种写法：
        // const vm = new Vue({
        //     // el:'#root', //第一种写法
        //     data:{
        //         name:'JOJO'
        //     }
        // })
        // vm.$mount('#root')//第二种写法

        //data的两种写法：
        new Vue({
            el:'#root', 
            //data的第一种写法：对象式
            // data:{
            //     name:'JOJO'
            // }
            //data的第二种写法：函数式
            data(){
                return{
                    name:'JOJO'
                }
            }
        })
    </script>
```

#### 1.6 MVVM模型

+ M：模型（Model），data中的数据
+ V：视图（View），模板代码
+ VM：视图模型（ViewModel），Vue实例

1. **data**中所有的属性最后都出现在**vm**身上
2. **vm**身上所有的属性 以及 **Vue原型**身上所有的属性，在**Vue模板**中都可以直接使用

![](https://img-blog.csdnimg.cn/img_convert/ac43527b06defd324a702aea4f7940a0.png)

#### 1.7 数据代理

通过一个对象代理对另一个对象的属性的操作

1. Vue中的数据代理通过vm对象来代理data对象中属性的操作（读/写）
2. Vue中数据代理能够更加方便的操作data中的数据   **vm.name / vm.address**
3. 基本原理：
   + 通过object.defineProperty()把data对象中的所有属性添加到vm上
   
     + ```
       Object.defineProperty(vm, 'name', {
       	get() {
       		return vm._data.name
       	},
       	set(value) {
       		vm._data.name = value
       	}
       })
       ```
   
   + 为每一个添加到vm上的属性都指定一个 getter / setter
   + 在 getter / setter 内部去操作data中对应的属性

![](https://img-blog.csdnimg.cn/img_convert/1fbebd52e39fa5a97210ee65d0a58069.png)

#### 1.8 事件处理

##### 1.8.1 事件的基本用法

1. 使用 **v-on:click** 或 **@xxx** 绑定事件，xxx代表事件名
2. 事件的回调需要配置在 **methods对象** 中，最终会在 vm 上
3. methods 中配置的函数，==**不要使用箭头函数**，否则this指向的不是vm
4. methods中配置的函数都是被 Vue 所管理的函数，this指向的时 vm 或者 组件实例对象
5. `@click="demo"`和`@click="demo($event)"`效 果一致，但后者可以传参

```html
<body>
    <div id="root">
        <h2>hello,{{name}}</h2>
        <button v-on:click="showInfo1">点我提示信息1</button>
        <button @click="showInfo2($event,66)">点我提示信息2</button>
    </div>
    <script>
        Vue.config.productionTip = false 
        new Vue({
            el:'#root', 
            data:{
                name:'JOJO'
            },
            methods:{
                showInfo1(event){
                    console.log(event)
                },
                showInfo2(event,num){
                    console.log(event,num)
                }
            }
        })
    </script>
</body>
```

##### 1.8.2 事件修饰符

1. prevent：阻止默认事件（常用）
2. stop：阻止事件冒泡（常用）
3. once：事件只触发一次（常用）
4. capture：使用事件的捕获模式
5. self：只有 event.target 是当前操作的元素时才触发事件
6. passive：事件的默认行为立即执行，无需等待时间回调执行完毕

> 修饰符可以连续写，@click.prevent.stop='showInfo'

```html
<div id="root">
    <h2>欢迎来到{{name}}学习</h2>
    <!-- 阻止默认事件 -->
    <a href="http://www.atguigu.com" @click.prevent="showInfo">点我提示信息</a>

    <!-- 阻止事件冒泡 -->
    <div class="demo1" @click="showInfo">
        <button @click.stop="showInfo">点我提示信息</button>
    </div>

    <!-- 事件只触发一次 -->
    <button @click.once="showInfo">点我提示信息</button>

    <!-- 使用事件的捕获模式 -->
    <div class="box1" @click.capture="showMsg(1)">
        div1
        <div class="box2" @click="showMsg(2)">
            div2
        </div>
    </div>

    <!-- 只有event.target是当前操作的元素时才触发事件 -->
    <div class="demo1" @click.self="showInfo">
        <button @click="showInfo">点我提示信息</button>
    </div>

    <!-- 事件的默认行为立即执行，无需等待事件回调执行完毕 -->
    <ul @wheel.passive="demo" class="list">
        <li>1</li>
        <li>2</li>
        <li>3</li>
        <li>4</li>
    </ul>
</div>
</body>

	<script type="text/javascript">
		Vue.config.productionTip = false

		new Vue({
			el:'#root',
			data:{
				name:'尚硅谷'
			},
			methods:{
				showInfo(e){
					alert('同学你好！')
				},
				showMsg(msg){
					console.log(msg)
				},
				demo(){
					for (let i = 0; i < 100000; i++) {
						console.log('#')
					}
					console.log('累坏了')
				}
			}
		})
	</script>
```

##### 1.8.3 键盘事件

1. 系统修饰键（用法特殊）：ctrl、alt、shift、meta

​		配合keyup使用：按下修饰键的同时，再按下其他键，随后释放其他键，事件才被触发
​		配合keydown使用：正常触发事件

2. 可以使用keyCode去指定具体的按键，比如：@keydown.13="showInfo"，但不推荐这样使用

3. Vue.config.keyCodes.自定义键名 = 键码，可以自定义按键别名

#### 1.9 计算属性

1. 定义：要用的属性不存在，需要通过**已有属性计算得来**
2. 原理：底层借助了 Object.defineProperty() 方法提供的 getter 和 setter
3. get函数在**初次读取时会执行一次**；**依赖的数据发生改变时会被再次调用**
4. 相较于methods实现，computed内部有**缓存机制**，效率更高

+ 计算属性最终会出现在vm上，直接读取使用

+ 如果计算属性需要被修改，必须使用setter函数去响应修改，且set中要引起计算时所依赖的数据发生改变

+ 如果**计算属性确定不考虑修改**则可以使用计算属性的简写形式

  ```js
  new Vue({
      el:'#root', 
      data:{ 
          firstName:'张',
          lastName:'三'
      },
      computed:{
      	fullName(){
  		    return this.firstName + '-' + this.lastName
      	}
      }
  })
  ```

#### 1.10 监视属性

**监视属性watch**

1. 当被监视的属性变化时，回调函数自动调用执行相关操作

2. 监视的属性 **必须存在** 才能进行监视

3. 两种监视写法：创建**Vue**实例时添加watch；**vm.$watch**

4. **深度监视**： 配置 **deep: true** 可以监测对象内部值的改变（多层）

   + **Vue**自身可以监测对象内部值的改变，但是watch默认不可以
   + 使用watch时根据监视数据的具体结构决定是否采取深度监视

5. **computed**和**watch**之间的区别：

   + computed能完成的功能，watch都可以完成
   + watch能完成的功能，computed不一定能完成，例如：watch可以进行异步操作

   **两个重要的小原则：**

   + 所有被Vue管理的函数，最好写成普通函数，这样this的指向才是vm或组件实例对象
   + 所有不被Vue所管理的函数（定时器的回调函数、ajax的回调函数等、Promise的回调函数），最好写成箭头函数，这样this的指向才是vm 或 组件实例对象。

```js
<script type="text/javascript">
    Vue.config.productionTip = false
    const vm = new Vue({
        el: "#root",
        data: {
            isHot: true,
            number: {
                a: 1,
                b: 2
            }
        },
        computed: {
            info() {
                return this.isHot ? '炎热' : '真的超级热'
            }
        },
        methods: {
            changeWeather() {
                this.isHot = !this.isHot;
            },
            add() {
                this.number.a++;
            }
        },
        watch: {
            isHot: {
                immediate: true,  // 初始时让handler调用一下
                // isHot发生改变时调用
                handler(newValue, oldValue) {
         		console.log('isHot被修改了', this.isHot, newValue, oldValue)
                }
            }
            //简写  isHot直接当作handler使用
            /* isHot(newValue, oldValue){
         		 console.log('isHot被修改了', this.isHot, newValue, oldValue)
            } */
        }
    })
    vm.$watch('number.a', {
        deep: true,  // 深度监视
        handler() {
            console.log('a被修改了')
        }
    })
</script>
```

#### 1.11 样式绑定

1. class样式：

+ 写法： **class='xxx'**, xxx可以是字符串、对象、数组
+ 字符串写法适用于：类名不确定，要动态获取
+ 对象写法适用于：要绑定多个样式，个数不确定，名字也不确定
+ 数组写法适用于：要绑定多个样式，个数确定，名字也确定，但不确定用不用

2. style样式

+ `:style="{fontSize: xxx}"`其中xxx是动态值
+ `:style="[a,b]"`其中a、b是样式对象

```html
<body>
    <div id="root">
        <!-- 绑定class样式  -- 字符串写法，  适用于样式的类名不确定，需要动态指定 -->
        <div class="basic" :class="mood" @click="changeMood">{{name}}</div><br><br>

        <!-- 绑定class样式--数组写法，适用于：要绑定的样式个数不确定、名字也不确定  -->
        <div class="basic" :class="classArr" @click="changeMood">{{name}}</div><br><br>

        <!-- 绑定class样式--对象写法，适用于：要绑定的样式个数确定、名字也确定，但要动态决定用不用 -->
        <div class="basic" :class="classObj" @click="changeMood">{{name}}</div><br><br>

        <div class="basic" :style="{fontSize: fsize + 'px'}">{{name}}</div>
    </div>
</body>

<script type="text/javascript">
    Vue.config.productionTip = false
    new Vue({
        el: '#root',
        data: {
            name: 'woshinidie',
            mood: 'normal',
            classArr: ['atguigu1', 'atguigu2', 'atguigu3'],
            classObj: {
                atguigu1: false,
                atguigu2: true,
            },
            fsize: 42
        },
        methods: {
            changeMood() {
                const arr = ['happy', 'sad', 'normal']
                const r = Math.floor(Math.random() * 3)
                this.mood = arr[r]
            }
        },
    })
</script>
```

#### 1.12 条件渲染

1. v-if：

   + `v-if="表达式"`

   + `v-else-if="表达式"`

   + `v-else`

   + 以上三者可以结合使用，但是结构不可以被打断

   + 适用于切换频率较低的场景

   + 不展示DOM元素直接被移除

2. v-show
   + v-show="表达式"
   + 适用于切换频率高的场景
   + 不展示的DOM元素为被移除，仅仅是使用样式隐藏掉
3. 使用v-if时，元素可能无法获取到，而使用v-show一定可以获取到元素

```html
<body>
    <div id="root">
       <h1>当前的n值是:{{n}}</h1>
       <button @click="n++">点我n+1</button>
       <!-- 使用 v-show 做条件渲染 不显示的情况下该节点依然存在-->
       <h2 v-show="n === 1">Angular</h2>
       <h2 v-show="n === 2">React</h2>
       <h2 v-show="n === 3">Vue</h2>

       <!-- 使用 v-if 做条件渲染 如果不显示，该节点会直接消失-->
       <h2 v-if="false">欢迎来到{{name}}</h2>
       <h2 v-if="1 == 1">欢迎来到{{name}}</h2>

       <!-- v-else-if 和 v-else -->
       <h2 v-if="n === 1">Angular</h2>
       <h2 v-else-if="n === 2">React</h2>
       <!-- v-else 在其他条件都不符合的情况下直接执行v-else -->
       <!-- v-if 和 v-else-if 之间不能存在其他结构，不能阻断 -->
       <h2 v-else>Vue</h2>
       
       <!-- template 与 v-if结合使用 template该节点不会显示-->
       <template v-if="n === 1">
            <h2>你好</h2>
            <h2>who are you</h2>
       </template>
    </div>
</body>

<script type="text/javascript">
    Vue.config.productionTip = false
    new Vue({
        el: '#root',
        data:{
            name: 'woshinidie',
            n: 0
        } 
    })
</script>
```

#### 1.13 列表渲染

1. v-for指令：

   + 用于展示列表数据
   + 语法：`<li v-for="(item, index) in xxx" :key="yyy">`，其中key可以是index，也可以是遍历对象的唯一标识
   + 可遍历：**数组、对象、字符串、指定次数**

2. **[react](https://so.csdn.net/so/search?q=react&spm=1001.2101.3001.7020)、vue中的key有什么作用？（key的内部原理）**   

   + 虚拟DOM中 key 的作用：key 是虚拟DOM中对象的标识，当数据发生变化时，Vue 会根据 【新数据】生成【新虚拟DOM】，随后 Vue进行【新虚拟DOM】与【旧虚拟DOM】的差异比较 （**diff算法**），比较规则如下：
     + 旧虚拟DOM中找到了与新虚拟DOM相同的 key：
       + 若虚拟DOM中内容没有改变，则直接使用之前的真实DOM
       + 若虚拟DOM中内容变了，则生成新的真实DOM，随后替换掉页面中之前的真实DOM
     + 旧虚拟DOM中未找到与新虚拟DOM相同的key ：
       + 创建新的真实DOM ，随后渲染到页面
   + 用 index索引值 作为 key 引发的问题：
     + 若对数据进行逆序添加、逆序删除等破坏顺序操作：会产生没有必要的真实DOM更新，界面效果没有问题，但是效率低下
     + 若结构中还包含着 **输入类的DOM**：会产生错误的DOM更新，界面有问题
   + 开发中如何选择key
     + 最好使用每条数据的唯一标识作为key，比如id、手机号、身份证号、学号等唯一值
     + 如果不存在对数据的逆序添加、逆序删除等破坏顺序的操作，仅用于渲染列表，使用index作为key是没有问题的

   ![](https://img-blog.csdnimg.cn/img_convert/c81ae98624c2e1a32b7ced171d9089de.png)

```html
<body>
    <div id="root">
        <!-- 遍历数组 -->
        <h2>人员列表</h2>
        <button @click="add">点击添加人员</button>
        <ul>
            <li v-for="(p, index) in persons" :key="p.id">
                {{p.name}}-{{p.age}} <input type="text">
            </li>
        </ul>
    </div>
</body>

<script type="text/javascript">
    Vue.config.productionTip = false
    new Vue({
        el: '#root',
        data: {
            persons: [
                { id: '001', name: '张三', age: 18 },
                { id: '002', name: '李四', age: 45 },
                { id: '003', name: '王五', age: 35 }
            ]
        },
        methods: {
            add(){
                const p = {id:'004', name:'隔壁老王', age: 24}
                this.persons.unshift(p)
            }
        }
    })
</script>
```

#### 1.14 Vue监视数据

Vue监视数据的原理：

1. Vue 监视 data 中**所有层次**的数据
2. 如何监测**对象**中的数据？

​	通过 setter 实现监视，且在new Vue时就要传入要监测的数据

+ 对象中后追加的属性， Vue默认不做响应式处理
+ 如需给后添加的属性做响应式，需要使用如下两种API：
  + **Vue.set(target, propertyName/index, value)**
  + **vm.$set(target, propertyName/index, value)**

3. 如何监测**数组**中的数据？

​	通过包裹数组更新元素的方法实现，**数组元素的响应式通过原型链上数组方法操作数组**

+ 调用数组原型链上的原生方法对数组进行更新
+ 重新解析模板更新页面

4. 在Vue修改数组中的某个元素一定要用如下方法：

+ 使用这些API：`push()`、`pop()`、`shift()`、`unshift()`、`splice()`、`sort()`、`reverse()`
+ `Vue.set()` 或 `vm.$set()`

**特别注意**：`Vue.set()` 和 `vm.$set()` 不能给vm 或 vm的根数据对象（data等） 添加属性

```html
<body>
    <div id="root">
        <h1>学生信息</h1>
        <button @click="student.age++">年龄+1岁</button>
        <button @click="addSex">添加性别属性，默认值：男</button>
        <button @click="student.sex = '未知' ">修改性别</button>
        <h3>姓名：{{student.name}}</h3>
        <h3>年龄：{{student.age}}</h3>
        <h3 v-if="student.sex">性别：{{student.sex}}</h3>
        <h3>爱好：</h3>
        <button @click="addHobby">添加一个爱好</button><br>
        <button @click="updateHobby">修改第一个爱好为：开车</button><br />
        <ul>
            <li v-for="(h,index) in student.hobby" :key="index">
                {{h}}
            </li>
        </ul>
        <h3>朋友们：</h3>
        <button @click="addFriend">添加一个朋友</button><br>
        <button @click="updateFirstFriendName">修改第一个朋友的名字为：张三</button>
        <ul>
            <li v-for="(f,index) in student.friends" :key="index">
                {{f.name}}--{{f.age}}
            </li>
        </ul>
    </div>
</body>

<script type="text/javascript">
    Vue.config.productionTip = false //阻止 vue 在启动时生成生产提示。

    const vm = new Vue({
        el: '#root',
        data: {
            student: {
                name: 'tom',
                age: 18,
                hobby: ['抽烟', '喝酒', '烫头'],
                friends: [
                    { name: 'jerry', age: 35 },
                    { name: 'tony', age: 36 }
                ]
            }
        },
        methods: {
            addSex() {
                // Vue.set(this.student, 'sex', '男')
                this.$set(this.student, 'sex', '男')
            },
            addFriend() {
                this.student.friends.unshift({ name: 'hh', age: '22' })
            },
            updateFirstFriendName() {
                this.student.friends[0].name = '张三'
                // 此时的name属于响应式对象，有对应的getter和setter
            },
            addHobby() {
                this.student.hobby.unshift('学习')
            },
            updateHobby() {
                this.student.hobby.splice(0,1,'开车')
                this.$set(this.student.hobby, 0, '开车')
                // 从第0个开始，删除1个
            }

        },
    })
</script>
```

#### 1.15 收集表单数据

1. 若`<input type="text"/>`，v-model 收集的是value值，用户输入的内容就是value值
2. 若`<input type="radio"/>`，v-model 收集的是value的值，且要给标签配置value属性
3. 若`<input type="checkbox"/>`，
   + 没有配置value属性，那么收集的就是checked属性（勾选或未勾选，是布尔值）
   + 配置了value属性：
     + v-model的初始值是非数组，那么收集的就是checked
     + v-model的初始值是数组，那么收集的就是value组成的数组
4. v-model 的三个修饰符：

+ lazy：失去焦点之后再收集数据，更新vm
+ number：输入字符串转为有效的数字
+ trim：输入首尾空格过滤

#### 1.16 过滤器

1. 对要显示的**数据进行特定格式化再显示** （适用于一些简单逻辑的处理）
2. 语法：
   + 注册过滤器：**Vue.filter(name, callback) 或 new Vue { filters:{} }**
   + 使用过滤器：**{{ xxx | 过滤器名 }} 或 v-bind:属性=” xxx | 过滤器名“**
3. 过滤器可以接收额外参数，多个过滤器之间可以串联
4. 不会改变原始数据，而是产生新的对应的数据

```html
<body>
    <div id="root">
        <h2>当前时间戳：{{time}}</h2>
        <h2>计算属性转换后时间：{{formatTime}}</h2>
        <h2>方法转换后时间：{{getFormatTime()}}</h2>
        <!-- 过滤器传参 -->
        <h2>过滤器转换后时间：{{time | timeFormater('YYYY年-MM月-DD日 HH:mm:ss')}}</h2>
        <h2>截取年月日：{{time | timeFormater() | slice}}</h2>
    </div>
</body>

<script type="text/javascript">
    Vue.config.productionTip = false
    Vue.filter('slice', (value) => {
        return value.slice(0, 15)
    })

    const vm = new Vue({
        el: '#root',
        data: {
            time: Date.now()
        },
        computed: {
            formatTime() {
                return dayjs(this.time).format('YYYY-MM-DD HH:mm:ss')
            }
        },
        methods: {
            getFormatTime() {
                return dayjs(this.time).format('HH:mm:ss')
            }
        },
        filters: {
            timeFormater(value, str) {
                return dayjs(value).format(str)
            }
        }
    })
</script>
```

#### 1.17 内置指令

+ **v-bind**：单向绑定解析表达式，可简写为 :
+ **v-model**：双向数据绑定
+ **v-for**：遍历数组 / 对象 / 字符串
+ **v-on**：绑定事件监听，可简写为@
+ **v-if**：条件渲染（动态控制节点是否存存在）
+ **v-else**：条件渲染（动态控制节点是否存存在）
+ **v-show**：条件渲染 (动态控制节点是否展示)

##### 1.17.1 v-text指令

向其所在的节点中渲染文本内容，不同于插值语法，**v-text会替换掉节点中的内容**，{{xx}}则不会

##### 1.17.2 v-html指令

1. 向指定节点中渲染包含html结构的内容
2. 与插值语法的区别：
   + **v-html会替换掉节点中所有的内容**，{{xx}}则不会
   + **v-html可以识别html结构**
3. v-html存在安全性问题！
   + 在网站上**动态渲染任意HTML**是非常危险的，容易导致XSS攻击
   + 一定要在**可信的内容中使用v-html，永远不要在用户提交内容上使用**

##### 1.17.3 v-cloak指令（没有值）

1. 本质是一个特殊属性，**Vue实例创建完毕并接管容器**后会删掉该属性

   ​	 `<h2 v-cloak>haha</h2>`

2. 使用css配合v-cloak可以解决网速慢时页面显示Vue未渲染页面的问题

​	css的属性选择器 `[v-cloak]{ display: none;}`

##### 1.17.4 v-once指令

1. **v-once所在节点在初次动态渲染后就视为静态内容**
2. 以后数据的改变不会引起v-once所在结构的更新，可以用于优化性能

##### 1.17.5 v-pre指令

1. **跳过其所在节点的编译过程**
2. 可以利用v-pre跳过：没有使用指令语法、插值语法的节点，加快编译

#### 1.18 自定义指令

1. 定义语法

+ 局部指令：**`new Vue({directives: { 指令名： 配置对象} })` 或者 `new Vue({directives: {指令名：回调函数}})`**
+ 全局指令：Vue.directive('指令名'，配置对象 / 回调函数)

2. 配置对象中常用的三个回调：

+ **bind:** 指令与元素成功绑定时调用
+ **inserted:** 指令所在元素被插入页面时调用
+ **update:** 指令所在模板结构被重新解析时调用

3. 备注：

+ 指令定义是不加v-，使用时加上v-
+ 指令名若是多个单词需要使用  **v-big-number**  形式，不能使用小驼峰命名

```html
<body>
	<div id="root">
		<h2>当前的n值：<span v-text="n"></span></h2>
		<h2>放大10倍后的n值：<span v-big="n"></span></h2>
		<button @click="n++">点我n+1</button>
		<hr>
		<input type="text" v-fbind:value="n">
	</div>
</body>

<script type="text/javascript">
	Vue.config.productionTip = false
	// 定义全局指令
	Vue.directive('big', function (element, binding) {
		console.log('big', this)
		element.innerHTML = binding.value * 10
	})
})
	new Vue({
		el: '#root',
		data: {
			n: 1,
		},
		directives: {
			// big元素调用时机：1. 指令与元素成功绑定时（一上来） 2. 指令所在的模板被重新解析时
			big(element, binding) {
				console.log('big', this)
				element.innerHTML = binding.value * 10
			},
			fbind: {
				// 指令与元素成功绑定时
				bind(element, binding) {
					element.value = binding.value
				},
				// 指令所在元素被插入页面时，对元素做一个操作
				inserted(element, binding) {
					element.focus()
				},
				// 指令所在的模板被重新解析时
				update(element, bingding) {
					element.value = binding.value * 10
				},
			}
		}
	})
```

#### 1.19 生命周期

1. 生命周期回调函数、生命周期函数、 **生命周期钩子**
2. 其是指Vue在关键时刻帮我们调用一些特殊名称的函数
3. 生命周期中 **this指向 vm 或 组件实例对象**
4. 常用的生命周期钩子：

+ `mounted`: 发送AJAX请求、启动定时器、绑定自定义事件、订阅消息等 **初始化操作**
+ `beforeDestroy`: 清除定时器、解绑自定义事件、取消订阅消息等 **收尾工作**

5. 销毁Vue实例：

+ 销毁后借助Vue开发者工具看不到任何信息
+ 销毁后自定义事件会失效，但原生DOM事件依然有效
+ 一般不会在`beforeDestroy`中操作数据，因为即使操作数据也不会出发更新流程

![https://img-blog.csdnimg.cn/img_convert/934db3f17c6daded6578bdf1a769d9dc.png](https://img-blog.csdnimg.cn/img_convert/934db3f17c6daded6578bdf1a769d9dc.png)

### Vue组件化编程

#### 2.1 模块与组件、模块化与组件化

1. 模块：向外提供特定功能的js程序，一般就是一个js文件
2. 组件： 用来实现 **局部** 功能的 **代码与资源** 的集合（html/css/js/image...）

#### 2.2 非单文件组件

1. Vue 中使用组件三步骤

+ 定义组件（创建组件）

  ```
  const school = Vue.extend( { } )
  ```

+ 注册组件

  全局注册 **Vue.component('组件名'，组件)**

  ```
  new vue({
  	...
  	components: {
  		school: school
  	}
  })
  ```

+ 使用组件 `<school></school>`



2. 定义组件时传入的**配置对象 options** 与 new Vue(options) 几乎一样

+ el 不需要写，最后都需要经过 vm 管理，由vm决定为哪个容器服务
+ **data 必须写成函数**，避免组件复用时数据存在引用关系



3. **组件名**的书写格式

+ 一个单词：首字母大小写都可以 school / School
+ 多个单词：（kebab-case命名）：my-school；（CamelCase）：MySchool （Vue脚手架支持）
+ 可以直接在**定义组件配置对象**中决定 **开发者工具**中 的名称 name: ''
+ 一个简写方式：`const school = Vue.extend(options)`可简写为：`const school = options`



4. 组件标签书写形式

+ 双标签： `<school></school>`
+ 单标签：`<school/>`（不使用脚手架会导致后续组件不能渲染）



5. **组件嵌套**



6. #### VueComponent

+ school组件本质时一个名为**`VueComponent`**的构造函数，且不是程序员定义的，是**`Vue.extend`**生成的

+ 我们只需要写**`<school/>`或`<school></school>`**，Vue解析时会帮我们创建school组件的实例对象，即Vue帮我们执行的：**`new VueComponent(options)`**
+ 每次调用 **Vue.extend** 返回的都是一个 **全新** 的 Vuecomponent
+ 组件配置中：data函数、methods中的函数、watch中的函数、computed中的函数 它们的**this均是VueComponent实例对象**
+ new Vue(options)配置中：data函数、methods中的函数、watch中的函数、computed中的函数 它们的**this均是Vue实例对象**
+ `VueComponent`的实例对象，以后简称vc（也可称之为：组件实例对象）



7. #### 一个重要的内置关系

+ **`VueComponent.prototype.__proto__ === Vue.prototype`**
+ 为什么要有这个关系：让组件实例对象（vc）可以访问到 Vue 原型上的属性、方法

![https://img-blog.csdnimg.cn/img_convert/8d8b0e2d24b9fd26ff33a600fe0afd7d.png](https://img-blog.csdnimg.cn/img_convert/8d8b0e2d24b9fd26ff33a600fe0afd7d.png)



#### 2.3 单文件组件

+ MySchool.Vue

```vue
<template>
    <div id='Demo'>
        <h2>学校名称：{{name}}</h2>
        <h2>学校地址：{{address}}</h2>
        <button @click="showName">点我提示学校名</button>
    </div>
</template>

<script>
    export default {
        name:'MySchool',
        data() {
            return {
                name:'尚硅谷',
                address:'北京'
            }
        },
        methods: {
            showName(){
                alert(this.name)
            }
        },
    }
</script>

<style>
    #Demo{
        background: orange;
    }
</style>
```

+ MyStudent.vue

```vue
<template>
    <div>
        <h2>学生姓名：{{name}}</h2>
        <h2>学生年龄：{{age}}</h2>
    </div>
</template>

<script>
    export default {
        name:'MyStudent',
        data() {
            return {
                name:'JOJO',
                age:20
            }
        },
    }
</script>
```

+ App.vue

```vue
<template>
    <div>
        <MySchool></MySchool>
        <MyStudent></MyStudent>
    </div>
</template>

<script>
    import MySchool from './MySchool.vue'
    import MyStudent from './MyStudent.vue'

    export default {
        name:'App',
        components:{
            MySchool,
            MyStudent
        }
    }
</script>
```

+ main.js

```js
import App from './App.vue'

new Vue({
    template:`<App></App>`,
    el:'#root',
    components:{App}
})
```

+ index.js

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>单文件组件练习</title>
</head>
<body>
    <div id="root"></div>
    <script src="../../js/vue.js"></script>
    <script src="./main.js"></script>
</body>
</html>
```



### Vue CLI脚手架

#### 3.1 初始化脚手架

1. 如果下载缓慢请配置 npm 淘宝镜像：`npm config set registry http://registry.npm.taobao.org`
2. 全局安装@vue/cli：`npm install -g @vue/cli`
3. 切换到你要创建项目的目录，然后使用命令创建项目：`vue create xxxx`
4. 选择使用vue的版本
5. 启动项目：`npm run serve`
6. 暂停项目：Ctrl+C

**分析脚手架结构**

```
.文件目录
├── node_modules 
├── public
│   ├── favicon.ico: 页签图标
│   └── index.html: 主页面
├── src
│   ├── assets: 存放静态资源
│   │   └── logo.png
│   │── component: 存放组件
│   │   └── HelloWorld.vue
│   │── App.vue: 汇总所有组件
│   └── main.js: 入口文件
├── .gitignore: git版本管制忽略的配置
├── babel.config.js: babel的配置文件
├── package.json: 应用包配置文件 
├── README.md: 应用描述文件
└── package-lock.json: 包版本控制文件
```

**render函数**

**使用render函数是因为使用了 vue.runtime.xxx.js 文件，该文件中不包含模板解析器**

1. vue.js 与 vue.runtime.xxx.js的区别：

   + vue.js 是完整版的 Vue，包含：核心功能+模板解析器


   + vue.runtime.xxx.js 是运行版的 Vue，只包含核心功能，没有模板解析器


2. 因为 vue.runtime.xxx.js 没有模板解析器，所以不能使用 template 配置项，需要使用 render函数接收到的createElement 函数去指定具体内容

#### 3.2 ref属性

1. 被用来 **给元素或者子组件注册引用信息** （id的替代者）
2. 应用在**html标签**上时获取的是**真实的DOM元素**，应用在**组件标签**上获取的是 **组件实例对象（VC）**
3. 使用方式：
   + 打标识 `<h1 ref="xxx"></h1>` 或 `<School ref="xxx"></School>`
   + 获取： `this.$refs.xxx`

#### 3.3 props配置项

1. 目标：**让组件接收外部传入的数据**

```vue
<template>
    <div>
        <Students name='法外狂徒' sex='张嘉文' :age='18'/>
    </div>
</template>

<script>
import Students from "./components/Students.vue";
export default {
    name: "App",
    components: {
        Students
    },
};
</script>
```

2. 传递数据： `<Students name='法外狂徒' sex='张嘉文' :age='18'/>`
3. 接收数据：
   + 只接收不限制： `props:['name']`
   + 限制数据类型：`props: { name: String }`
   + 限制数据类型，限制必要性：`props: { name: {required:true, defeault:''}} `

**props 是只读的， Vue底层会监测对于props的修改，如果进行的修改就会发出警告，若是需要修改可以在 data 中使用 this.xxx 赋值给新属性，修改新属性的值**

#### 3.4 mixin混合配置项

1. 功能：可以将多个组件**共用的配置提**取成一个混入对象
2. 使用方式：
   + 定义并且暴露混合： `export const mixin = { data(){}, methods: {}}`
   + 使用混合：
     + 引入混合：`import { mixin } from '../mixin.js'`
       + 全局混合：`Vue.mixin( mixin )`
       + 局部混合：`mixins: [ mixin ]`
3. 组件中与混合对象有同名选项时，这些选项将以恰当的方式合并，**发生冲突时以组件优先**

同名生命周期钩子将会合并为一个数组，钩子函数都被调用，另外混入对象的钩子在组件自身钩子之前调用

#### 3.5 plugin插件

1. 用于增强Vue
2. 包含 install 方法的一个对象，install 的第一个参数是Vue，第二个以后的参数是插件使用需要传递的参数
3. 使用插件：在入口文件`main.js`中 `Vue.use(plugin)`

```js
plugin.install = function (Vue, options) {
        // 1. 添加全局过滤器
        Vue.filter(....)
    
        // 2. 添加全局指令
        Vue.directive(....)
    
        // 3. 配置全局混入
        Vue.mixin(....)
    
        // 4. 添加实例方法
        Vue.prototype.$myMethod = function () {...}
        Vue.prototype.$myProperty = xxxx
    }
```

#### 3.6 scoped样式

1. 让样式在局部生效，防止冲突
2.  `<style scoped>`

#### 3.7 TodoList案例

+ 组件化编码流程：
  1. 拆分静态组件：组件要按照功能点拆分， **命名不要与html元素冲突**
  2. 实现动态组件：考虑好数据的存放位置，数据是只在一个组件用还是一些组件都用
     + 一个组件使用：数据放在自身即可
     + 一些组件使用：数据放在共同的父组件上（**状态提升**）
  3. 实现交互：从绑定事件开始
+ **props**适用：
  1. 父组件到子组件的通信
  2. 子组件到父组件，要求**父组件要先给子组件传递一个函数**，子组件中输入参数调用函数

+ 使用 **v-model**要切记：其绑定的值不能是 props 传递的值，因为props传递的值是只读的
+ props 传过来的若是对象类型的值，修改对象中的属性时Vue不会报错，但是不建议操作props

#### 3.8 WebStorage

1. 存储内容大小一般支持5MB左右
2. 浏览器端通过 `window.sessionStorage` 和 `window.localStorage` 属性实现本地存储机制
3. 相关API：

+ `xxxStorage.setItem('key', 'value')`:该方法接收一个键和值作为参数，会把键值对添加到存储中，如果键名存在则更新对应的值
+ `xxxStorage.getItem('key')`:该方法接收一个键名作为参数，返回键名对应的值
+ `xxxStorage.removeItem('key')`：该方法接收一个键名作为参数，并把该键名从存储中删除
+ `xxxStorage.clear()`：该方法会清空存储中的所有数据

4. 注意事项

+ `SessionStorage`：存储的内容会随着浏览器窗口关闭而消失
+ `LocalStorage`:存储的内容，需要手动清除才会消失
+ `xxxStorage.getItem(xxx)`:如果 xxx 对应的 value 获取不到，那么`getItem()`的返回值是null
+ `JSON.parse(null)`的结果依然是null

#### 3.9 自定义事件

组件的**自定义事件**：

1. 一种组件间通信的方式，适用于： **子组件 ==> 父组件**

2. 使用场景：A是父组件，B是子组件，B想给A传数据，那么就要在A中给B绑定自定义事件（**事件的回调在A中**）

3. 绑定自定义事件：

   + ①：`<Students @atguigu="test">  /  <Students v-on:atguigu="test">`

   + ②：`<Students ref="student">`

      		`mounted(){ this.$refs.student.$on('atguigu', data) }`

   + 若想让事件只触发一次可以将 `$on换成$once` 或者 `@atguigu.once`

4. 触发自定义事件（在子组件中触发）： `this.$emit("atguigu", data)`

5. 解绑自定义事件（在子组件中解绑）： `this.$off("atguigu")`

6. 组件上可以绑定原生DOM事件，但是需要在时间名后加native修饰符 `@click.native=""`

7. **Warning**：通过`this.$refs.xxx.$on('atguigu',回调)`绑定自定义事件时，回调 **要么配置在methods中，要么使用箭头函数 使得this指向依然为当前组件实例对象**

#### 3.10 全局事件总线（GlobalEventBus）

**全局事件总线是一种可以在任意组件间通信的方式，本质上就是一个对象。它必须满足以下条件：1. 所有的组件对象都必须能看见他 2. 这个对象必须能够使用`$on`、`$emit`和`$off`方法去绑定、触发和解绑事件**

1. 一种组件间通信的方式，适用于**任意组件间通信**
2. 安装全局事件总线：

```js
new Vue({
	...
	beforeCreate(){
		Vue.prototype.$bus = this // 安装全局事件总线，$bus就是当前应用的vm
	}
})
```

3. 使用事件总线：

+ 接收数据：A组件想接收数据则在A组件中给`$bus`绑定自定义事件，事件的回调留在A组件自身

```js
export default {
    methods(){
        demo(data){...}
    },
    ...
    mounted() {
        this.$bus.$on('xxx',this.demo)
    }
}
```

+ 提供数据：发送数据组件内`this.$bus.$emit('xxx', data)`

4. 最好在beforeDestroy钩子中，用$off去解绑当前组件所用到的事件

#### 3.11 消息订阅与发布

1. 一种组件间通信的方式，适用于**任意组件间通信**
2. 使用步骤：

+ 安装pubsub：`npm i pubsub-js`
+ 引入：`import pubsub from 'pubsub-js'`
+ 接收数据：A组件想接收数据，则在A组件中订阅消息，订阅的回调留在A组件自身

```js
export default {
    methods(){
        demo(data){...}
    }
    ...
    mounted() {
		this.pid = pubsub.subscribe('xxx',this.demo)
    }
}
```

+ 提供数据：`pubsub.publish('xxx',data)`
+ 最好在`beforeDestroy`钩子中，使用`pubsub.unsubscribe(pid)`取消订阅

#### 3.12 $nextTick

> **`$nextTick(回调函数)`可以将回调延迟到下次 DOM 更新循环之后执行**

$nextTick:

1. 语法：`this.$nextTick(function(){})`
2. 作用：在下一次DOM更新结束后执行其指定的回调
3. 使用时间点：**当改变数据后，要基于更新后的新DOM进行某些操作时，要在nextTick所指定的回调函数中执行**



### Vue中的Ajax

#### 4.1 Vue脚手架配置代理

方法一： 在`vue.config.js`中添加如下配置：

```
devServer:{
    proxy:"http://localhost:5000"
}
```

说明：

1. 优点：配置简单，请求资源时直接发给前端即可
2. 缺点：不能配置多个代理，不能灵活的控制请求是否走代理
3. 工作方式：若按照上述配置代理，当请求了前端不存在的资源时，那么该请求会转发给服务器 （优先匹配前端资源）

方法二：

```js
devServer: {
    proxy: {
      	'/api1': { // 匹配所有以 '/api1'开头的请求路径
        	target: 'http://localhost:5000',// 代理目标的基础路径
        	changeOrigin: true,
        	pathRewrite: {'^/api1': ''}
      	},
      	'/api2': { // 匹配所有以 '/api2'开头的请求路径
        	target: 'http://localhost:5001',// 代理目标的基础路径
        	changeOrigin: true,
        	pathRewrite: {'^/api2': ''}
      	}
    }
}
// axios.get('http://localhost:8080/api1/students')
// changeOrigin设置为true时，服务器收到的请求头中的host为：localhost:5000
// changeOrigin设置为false时，服务器收到的请求头中的host为：localhost:8080
```

说明：

1. 优点：可以配置多个代理，且可以灵活的控制请求是否走代理
2. 缺点：配置略微繁琐，请求资源时必须加前缀

#### 4.2 Vue-resource

vue项目中常用的两个Ajax库：

+ axios：通用的Ajax请求库，官方推荐，效率高

+ vue-resource：vue插件库

  ```js
  //main.js文件中引用vue-resource插件
  import vue from 'vue'
  import App from './App.vue'
  import vueResource from 'vue-source'
  
  vue.use(vueResource)
  new Vue({
  	el:'#app',
  	render: h => h(app)
  })
  
  // 使用将 axios 换成 this.$http
  ```

#### 4.3 插槽slot

作用：让父组件可以向子组件指定位置插入html结构，适用于父组件 > 子组件间的通信

1. 默认插槽

```
父组件中：
        <Category>
           	<div>html结构1</div>
        </Category>
子组件中：
        <template>
            <div>
               	<slot>插槽默认内容...</slot>
            </div>
        </template>
```

2. 具名插槽

```
父组件中：
        <Category>
            <template slot="center">
             	 <div>html结构1</div>
            </template>

            <template v-slot:footer>
               	<div>html结构2</div>
            </template>
        </Category>
子组件中：
        <template>
            <div>
               	<slot name="center">插槽默认内容...</slot>
                <slot name="footer">插槽默认内容...</slot>
            </div>
        </template>
```

3. 作用域插槽

+ **数据在组件自身，但是根据数据生成的结构需要组件的使用者来决定**（games数据在Category组件中，但使用数据所遍历出来的结构由App组件决定）

```
父组件中：
		<Category>
			<template scope="scopeData">
				<!-- 生成的是ul列表 -->
				<ul>
					<li v-for="g in scopeData.games" :key="g">{{g}}</li>
				</ul>
			</template>
		</Category>

		<Category>
			<template slot-scope="scopeData">
				<!-- 生成的是h4标题 -->
				<h4 v-for="g in scopeData.games" :key="g">{{g}}</h4>
			</template>
		</Category>
子组件中：
        <template>
            <div>
                <slot :games="games"></slot>
            </div>
        </template>
		
        <script>
            export default {
                name:'Category',
                props:['title'],
                //数据在子组件自身
                data() {
                    return {
                        games:['红色警戒','穿越火线','劲舞团','超级玛丽']
                    }
                },
            }
        </script>
```



### VUEX

#### 5.1 Vuex是什么？

+ 专门在 Vue 中实现 **集中式状态（数据）**管理的一个 **Vue插件**，对vue应用中多个组件的共享状态进行集中式的管理（读/写），也是一种组件间通信的方式，适用于任意组件间通信
+ Vuex使用背景：
  + 多个组件依赖同一状态
  + 来自不同组件的行为需要变更同一状态

#### 5.2 Vuex工作原理图

![](https://img-blog.csdnimg.cn/img_convert/11a966479bad6de2bc521d30b90d391f.png)

#### 5.3 搭建Vuex环境

1. 下载Vuex： `npm i vuex`

2. 创建 `src/store/store.js`

```js
//引入Vue核心库
import Vue from 'vue'
//引入Vuex
import Vuex from 'vuex'
//应用Vuex插件
Vue.use(Vuex)
   
//准备actions对象——响应组件中用户的动作、处理业务逻辑，接收来自 dispatch 的值
const actions = {
    increaseOdd(context, value) {
        if(context.state.sum % 2) {
            context.commit("IncreaseOdd", value);
        }
    },
}
//准备mutations对象——修改state中的数据，接收来自 commit 的数据
const mutations = {
    ADD(state, value) {
        console.log("mutations中的ADD被调用了", state, value);
        state.sum += value;
    },
}
//准备state对象——保存具体的数据
const state = {}
   
//创建并暴露store
export default new Vuex.Store({
   	actions,
   	mutations,
   	state
})
```

3. 在`src/main.js`中创建vm时传入`store`配置项：

```js
import Vue from "vue"
import App from './App.vue'

//引入store
import store from './store'

Vue.config.productionTip = false
new Vue({
    el: '#app',
    render: h => h(App),
    store,
    beforeCreate(){
        // 安装全局事件总线，$bus就是当前的vm
        Vue.prototype.$bus = this
    }
})
```

**Vuex基本使用：**

+ 初始化数据 **state** ，配置 **actions, mutations**
  + actions中方法中传入两个参数 **context（miniStore），value（来自组件的值）**
  + mutations中方法传入两个参数 **state（数据）， value（来自actions方法传递的value）**
+ 组件中读取 vuex 中的数据： `$store.state.sum`
+ 组件中修改 vuex 中的数据：`this.$store.dispatch('actions中的方法名', 数据)`or`this.$store.commit('mutations中的方法名', 数据)`

> 若没有网络请求或其他业务逻辑，组件中可以越过 **actions**，即不写 dispatch，直接编写commit

#### 5.4 getters配置项

**getters**配置项的使用：

+ 当 **state** 中的数据需要经过加工后再使用时，可以使用 **getters** 加工
+ 在 `index.js`中追加 **getters** 配置

```js
const getters = {
	bigSum(state) {
		return state.sum * 10
	}
}
export default new Vuex.store({
	...
	getters
})
```

+ 组件中读取数据：`$store.getters.bigSum`

#### 5.5 mapState / mapGetters / mapActions / mapMutations

> 从vuex插件中引入四种映射方法
>
> ```js
> import {mapState,mapGetters,mapMutations,mapActions} from 'vuex'
> ```

1. **mapState / mapGetters**方法：用于帮助映射 `state/getters`中的数据

```js
computed: {
        // 借助 mapState/ mapGetters 生成计算属性（数组写法）
        ...mapState(['sum','school','subject']),
        ...mapGetters(['bigSum'])
        // 借助 mapState/ mapGetters 生成计算属性（对象写法）
        // ...mapState({sum:'sum',school:'school',subject: "subject"}),
        // ...mapGetters(bigSum:'bigSum')
    }
```

注意：使用数组写法时，键名和键值需要保持一致，对象写法中的键值对应state中的值

2. **mapActions / mapMutations**方法：用于生成`actions / mutations`中的方法，内部包含`$store.dispatch('XXX') / $store.commit('XXX')`

```js
methods: {
        /* increase() {
            this.$store.commit("ADD", this.n);
        }, 
        decrease() {
            //this.sum -= this.n
            this.$store.commit("SUBTRACT", this.n);
        },*/
        //借助 mapMutations 生成对应的方法，方法中调用commit联系mutations（对象写法）
        ...mapMutations({ increase: "ADD", decrease: "SUBTRACT" }),
        //借助 mapMutations 生成对应的方法，方法中调用commit联系mutations（数组写法）
        // 此时 方法命名在 模板中 与 mutations 中方法名一致
        // ...mapMutations(['ADD', 'SUBTRACT'])

        /* increaseOdd() {
            this.$store.dispatch("increaseOdd", this.n);
        },
        increaseWait() {
            this.$store.dispatch("increaseWait", this.n);
        }, */
        ...mapActions(["increaseOdd", "increaseWait"]),
    },
```

注意：此时参数的传递需要在模板字符串中绑定事件时传递好参数，否则参数是事件对象

#### 5.6 多组件共享数据

#### 5.7 模块化+命名空间

1. 修改 `index.js`，将属于一个组件的actions/mutations/state/getters放在一个对象中

```js
const countAbout = {
	namespaced:true,//开启命名空间
	state:{x:1},
    mutations: { ... },
    actions: { ... },
  	getters: {
    	bigSum(state){
       		return state.sum * 10
    	}
  	}
}

const personAbout = {
  	namespaced:true,//开启命名空间
  	state:{ ... },
  	mutations: { ... },
  	actions: { ... }
}

const store = new Vuex.Store({
  	modules: {
    	countAbout,
    	personAbout
  	}
})
```

2. 开启命名空间，组件中读取 **state** 数据：

```js
//方式一：自己直接读取
this.$store.state.personAbout.list
//方式二：借助mapState读取：
...mapState('countAbout',['sum','school','subject']),
```

3. 组件读取 **getters** 数据：

```js
//方式一：直接读取
this.$store.getters['personAbout/firstPersonName']
//方式二：借助mapGetters
...mapGetters('countAbout',['bigSum'])
```

4. 组件调用 **dispatch**

```js
//
this.$store.dispatch('personAbout/addPersonWang', value)
//
...mapActions("countAbout", ["increaseOdd", "increaseWait"]),
```

5. 组件调用 **commit**

```JS
//
this/$store.commit('personAbout/ADD_PERSON', value)
//
...mapMutations('countAbout',[increase:'ADD', decrease:'SUBTRACT'])
```



### Vue Router

#### 6.1 Vue-router 理解

+ vue的一个插件库，专门用来实现（single page web application，SPA）应用
  + **SPA单页web应用**，整个应用只有**一个完整的页面**，点击导航拦截**不会刷新页面**，只会做页面局部更新，数据需要通过 ajax 请求获取
+ **路由理解：**一个路由就是一组映射关系（key-value）
  + key 为路径，value 可能是 function 或 component
  + 后端路由：value 为 function ，用于处理客户端提交的请求，**服务器接收到一个请求时，根据请求路径找到匹配的函数来处理请求，返回响应数据**
  + 前端路由：value 为 component，用于展示页面内容，**当浏览器路径改变时，对应的组件就会显示**

#### 6.2 基本路由

> 安装vue-router: npm i vue-router

1. 创建 router 文件夹，创建 index.js文件 `src/router/index.js`

```js
//该文件专门用于创建整个应用的路由器
import VueRouter from "vue-router";
//引入组件
import Home from '../components/Home'
import About from '../components/About'

//创建并暴露一个路由器
export default new VueRouter({
    routes:[
        {
            path:'/about',
            component:About
        },
        {
            path:'/home',
            component:Home
        }
    ]
})
```

2. 入口文件main.js中引入 router 模块

```js
import Vue from 'vue'
import App from './App.vue'
import VueRouter from 'vue-router'
import router from './router'

Vue.config.productionTip = false
Vue.use(VueRouter)

new Vue({
    el:"#app",
    render: h => h(App),
    router
})
```

3. 实现切换： `<router-link active-class="active" to="/about">About</router-link>`
4. 指定展示位：`<router-view></router-view>`

#### 6.3 注意事项

+ 路由组件存放文件夹为 `pages`，一般组件通常存放在 `components`文件夹
+ 通过切换，’隐藏‘了的路由组件默认被销毁，需要的时候再去挂载
+ 每个组件都有自己的 `$route`（**路由规则**），里面存储着自己的路由信息
+ 整个应用只有一个 router， 可以通过组件内的 `$router` 属性获取

#### 6.4 多级路由

1. 配置路由规则，使用children配置项：

```js
routes:[
	{
		path:'/about',
		component:About,
	},
	{
		path:'/home',
		component:Home,
		children:[ //通过children配置子级路由
			{
				path:'news', //此处一定不要写：/news
				component:News
			},
			{
				path:'message', //此处一定不要写：/message
				component:Message
			}
		]
	}
]
```

2. 跳转（**填写完整路径**）`<router-link to='/home/news'>News</router-link>`

#### 6.5 路由的query参数

1. 参数传递：在<router-link>中的to属性传递query参数

```js
<!-- 跳转路由并携带query参数，to的字符串写法 -->
<router-link :to="`/home/message/detail?id=${m.id}&title=${m.title}`">{{m.title}}</router-link>
 <!-- 跳转路由并携带query参数，to的对象写法 -->
 <router-link :to="{
 	path:'/home/message/detail',
    query:{
    	id:m.id,
    	title:m.title
    	}
    }">
    {{m.title}}
</router-link>
```

2. 参数接收：

```
$route.query.id
$route.query.title
```

#### 6.6 命名路由

>给路由命名，方便路由跳转。

```js
<!--简化前，需要写完整的路径 -->
<router-link to="/demo/test/welcome">跳转</router-link>

<!--简化后，直接通过名字跳转 -->
<router-link :to="{name:'hello'}">跳转</router-link>

<!--简化写法配合传递参数 -->
<router-link 
	:to="{
		name:'hello',
		query:{
		    id:666,
            title:'你好'
		}
	}"
>跳转</router-link>
```

#### 6.7 路由的params参数

1. 配置路由时声明接收params参数，path路径后面使用占位符声明接收params参数
2. 传递参数利用to属性，特别注意：路由携带params参数时，若使用to的对象写法，则不能使用path配置项，必须使用name配置！

#### 6.8 路由的props配置

+ 作用：让路由组件更方便接收到参数

```js
{
	name:'xiangqing',
	path:'detail/:id',
	component:Detail,

	//第一种写法：props值为对象，该对象中所有的key-value的组合最终都会通过props传给Detail组件
	// props:{a:900}

	//第二种写法：props值为布尔值，布尔值为true，则把路由收到的所有params参数通过props传给Detail组件
	// props:true
	
	//第三种写法：props值为函数，该函数返回的对象中每一组key-value都会通过props传给Detail组件
	props(route){
		return {
			id:route.query.id,
			title:route.query.title
		}
	}
}
```

#### 6.9 路由跳转 replace 方法

1. 作用：**控制路由跳转时操作浏览器历史记录的模式**
2. 浏览器的历史记录两种写入方式：**push和replace**，push往栈内堆加历史记录，replace替换栈内当前历史记录，默认为push方式
3. 如何开启replace：**在<router-link replace></router-link>添加replace属性**

#### 6.10 编程式路由导航 

1. 作用：不借助 <router-link> 实现路由跳转，利用 `$router`路由中的push和replace属性实现
2. 具体编码：

```vue
this.$router.push({
	name:'xiangqing',
    params:{
        id:xxx,
        title:xxx
    }
})

this.$router.replace({
	name:'xiangqing',
    params:{
        id:xxx,
        title:xxx
    }
})
this.$router.forward() //前进
this.$router.back() //后退
this.$router.go() //可前进也可后退
```

#### 6.11 缓存路由组件

1. 作用：**让不展示的路由组件保持挂载，切换到其他组件时不被销毁**
2. 具体编码：

```vue
//缓存一个路由组件
<keep-alive include="News"> //include中写想要缓存的组件名，不写表示全部缓存
    <router-view></router-view>
</keep-alive>

//缓存多个路由组件
<keep-alive :include="['News','Message']"> 
    <router-view></router-view>
</keep-alive>
```

#### 6.12 两个新生命周期钩子 activated / deactivated

1. `activated / deactivated`是路由组件所独有的两个钩子，用于捕获路由组件的激活状态
2. 具体使用：
   1. `activated`路由组件被激活时触发
   2. `deactivated`路由组件失活时触发

#### 6.13 路由守卫

+ **对路由进行权限控制**
+ 分类：全局守卫、独享守卫、组件内守卫
+ 全局守卫：**在router配置文件index.js中配置全局守卫**

```js
//全局前置守卫：初始化时执行、每次路由切换前执行
router.beforeEach((to,from,next)=>{
	console.log('beforeEach',to,from)
	if(to.meta.isAuth){ //判断当前路由是否需要进行权限控制
		if(localStorage.getItem('school') === 'atguigu'){ //权限控制的具体规则
			next() //放行
		}else{
			alert('暂无权限查看')
		}
	}else{
		next() //放行
	}
})

//全局后置守卫：初始化时执行、每次路由切换后执行
router.afterEach((to,from) => {
	console.log('afterEach',to,from)
	if(to.meta.title){ 
		document.title = to.meta.title //修改网页的title
	}else{
		document.title = 'vue_test'
	}
})
```

+ 独享守卫：**在配置组件路由时配置独享守卫**

```js
beforeEnter(to,from,next){
	console.log('beforeEnter',to,from)
    if(localStorage.getItem('school') === 'atguigu'){
        next()
    }else{
        alert('暂无权限查看')
    }
}
```

+ 组件内守卫：**在组件内配置**

```js
//进入守卫：通过路由规则，进入该组件时被调用
beforeRouteEnter (to, from, next) {...},
//离开守卫：通过路由规则，离开该组件时被调用
beforeRouteLeave (to, from, next) {...},
```

#### 6.14. 路由器的两种工作模式

1. 对于一个url来说，什么是hash值？—— #及其后面的内容就是hash值

2. hash值不会包含在 HTTP 请求中，即：hash值不会带给服务器

3. hash模式：

​		地址中永远带着#号，不美观
​		若以后将地址通过第三方手机app分享，若app校验严格，则地址会被标记为不合法
​		兼容性较好

4. history模式：

​		地址干净，美观
​		兼容性和hash模式相比略差
​		应用部署上线时需要后端人员支持，解决刷新页面服务端404的问题
