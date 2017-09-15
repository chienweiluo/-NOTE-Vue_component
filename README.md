#Record #Vue #props #eventBus #emit() #v-on
<br/>
在學習Vue的時間中, 各component間的溝通間的觀念一時沒有好好釐清, 所以在這做為紀錄或是讓印象深刻<br/>
<br/>
<h3>組件component</h3>

---------------------------------------
註冊全局組件<br/>
  Vue.component('__TagName__',{ __options__ })   

    HTML
        <div id="example"><my-component></my-component></div>
    JS    
        Vue.component('my-component',{
            template:'<div>A Custom component! </div>'
        })
        
        new Vue({
            el: '#example'
        }

如果使用data屬性 必須是個function

## 1. Parent -> Child 輩分 

-----------------------

> 在 Vue.js 中，父子组件的关系可以总结为 props down, events up 。父组件通过 props 向下传递数据给子组件，子组件通过 events 给父组件发送消息。看看它们是怎么工作的。<br/>
>在一个良好定义的接口中尽可能将父子组件解耦是很重要的。这保证了每个组件可以在相对隔离的环境中书写和理解，也大幅提高了组件的可维护性和可重用性。<br/>
><br/><br/>
>解耦: 我認知為是各司其職. 話說這是在知乎上看到低耦合高內聚相關問答學的, 扯遠了 <br/>
<br/>
<img src="https://cn.vuejs.org/images/props-events.png"/>

組件的作用域是孤立的
意味者我們無法 也最好不要 在子模板直接使用父模板的資料 因為這樣很容易在大型或是稍微複雜的專案中, 降低程式碼的可使性
所以使用 **props**屬性來 **聲明它期待獲得的數據**

    Vue.component('child', {
    // 声明 props
    props: ['message'],
    // 就像 data 一样，prop 可以用在模板内
    // 同样也可以在 vm 实例中像 “this.message” 这样使用
    template: '<span>{{ message }}</span>'
    })

    <child message="hello!"></child>

    hello!

动态地绑定父组件的数据到子模板的props，与绑定到任何普通的HTML特性相类似，就是用 v-bind

<h4>單向數據流</h4>
prop 是单向绑定的：当父组件的属性变化时，将传导给子组件，但是不会反过来。这是为了防止子组件无意修改了父组件的状态——这会让应用的数据流难以理解.

另外，每次父组件更新时，子组件的所有 prop 都会更新为最新值。这意味着你不应该在子组件内部改变 prop 。如果你这么做了，Vue 会在控制台给出警告。

通常有两种改变 prop 的情况：

prop 作为初始值传入，子组件之后只是将它的初始值作为本地数据的初始值使用;

prop 作为需要被转变的原始值传入。

更确切的说这两种情况是:

定义一个本地数据，并且将 prop 的初始值设为本地数据的初始值。

定义一个基于 prop 值的计算属性。

注意在 JavaScript 中对象和数组是引用类型，指向同一个内存空间，如果 prop 是一个对象或数组，在子组件内部改变它会影响父组件的状态。

要指定验证规格，需要用对象的形式，而不能用字符串数组：

    Vue.component('example', {
      props: {
        // 基础类型检测 （`null` 意思是任何类型都可以）
        propA: Number,
        // 多种类型
        propB: [String, Number],
        // 必传且是字符串
        propC: {
          type: String,
          required: true
        },
        // 数字，有默认值
        propD: {
          type: Number,
          default: 100
        },
        // 数组／对象的默认值应当由一个工厂函数返回
        propE: {
          type: Object,
          default: function () {
            return { message: 'hello' }
          }
        },
        // 自定义验证函数
        propF: {
          validator: function (value) {
            return value > 10
          }
        }
      }
    })
      
## 2. Siblings 輩分間 - EventBus

----------

<img src="https://camo.githubusercontent.com/4bcd2174fc43d4b9d531cf8540eb40c7403ae8de/687474703a2f2f696d6731372e706f636f2e636e2f6d79706f636f2f6d7970686f746f2f32303135303530312f32312f31373335363236333432303135303530313231323134383038312e706e673f31303234783338335f313230" />

    ar bus = new Vue()
    // 触发组件 A 中的事件(发布消息)
    bus.$emit('id-selected', 1)
    // 在组件 B 创建的钩子中监听事件（订阅消息）
    bus.$on('id-selected', function (id) {
    // ...
    })
    在更多复杂的情况下，你应该考虑使用专门的 状态管理模式.(vuex)

## 3. Child -> Parent 輩分 - 自定義事件 

-------------------
$emit()  觸發事件 
$on() 監聽事件

父组件可以在使用子组件的地方直接用 v-on 来监听子组件触发的事件

自定义事件可以用来创建自定义的表单输入组件，使用 v-model 来进行数据双向绑定


source: [Vue文檔](https://cn.vuejs.org/v2/guide/components.html#%E4%BD%BF%E7%94%A8-v-on-%E7%BB%91%E5%AE%9A%E8%87%AA%E5%AE%9A%E4%B9%89%E4%BA%8B%E4%BB%B6)



      
