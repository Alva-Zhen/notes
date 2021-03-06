1. React 中 keys 的作用是什么？

    Keys 是 React 用于追踪哪些列表中元素被修改、被添加或者被移除的辅助标识。
    ```
    render () {
        return (
            <ul>
                {this.state.todoItems.map(({item, key}) => {
                    return <li key={key}>{item}</li>
                })}
            </ul>
        )
    }
    ```

    在开发过程中，我们需要保证某个元素的 key 在其同级元素中具有唯一性。在 React Diff 算法中 React 会借助元素的 Key 值来判断该元素是新近创建的还是被移动而来的元素，从而减少不必要的元素重渲染。此外，React 还需要借助 Key 值来判断元素与本地状态的关联关系，因此我们绝不可忽视转换函数中 Key 的重要性。

2. 调用 setState 之后发生了什么

    在代码中调用 setState 函数之后，React 会将传入的参数对象与组件当前的状态合并，然后触发所谓的调和过程（Reconciliation）。经过调和过程，React 会以相对高效的方式根据新的状态构建 React 元素树并且着手重新渲染整个 UI 界面。在 React 得到元素树之后，React 会自动计算出新的树与老树的节点差异，然后根据差异对界面进行最小化重渲染。在差异计算算法中，React 能够相对精确地知道哪些位置发生了改变以及应该如何改变，这就保证了按需更新，而不是全部重新渲染。

2. react 生命周期函数

    * 初始化阶段：

        getDefaultProps:获取实例的默认属性
        getInitialState:获取每个实例的初始化状态
        componentWillMount：组件即将被装载、渲染到页面上
        render:组件在这里生成虚拟的 DOM 节点
        componentDidMount:组件真正在被装载之后
    
    * 运行中状态：

        componentWillReceiveProps:组件将要接收到属性的时候调用
        shouldComponentUpdate:组件接受到新属性或者新状态的时候（可以返回 false，接收数据后不更新，阻止 render 调用，后面的函数不会被继续执行了）
        componentWillUpdate:组件即将更新不能修改属性和状态
        render:组件重新描绘
        componentDidUpdate:组件已经更新

    * 销毁阶段：

        componentWillUnmount:组件即将销毁

3. shouldComponentUpdate 是做什么的，（react 性能优化是哪个周期函数？）

    shouldComponentUpdate 这个方法用来判断是否需要调用 render 方法重新描绘 dom。因为 dom 的描绘非常消耗性能，如果我们能在 shouldComponentUpdate 方法中能够写出更优化的 dom diff 算法，可以极大的提高性能。

4. 为什么虚拟 dom 会提高性能?

    虚拟 dom 相当于在 js 和真实 dom 中间加了一个缓存，利用 dom diff 算法避免了没有必要的 dom 操作，从而提高性能。
    用 JavaScript 对象结构表示 DOM 树的结构；然后用这个树构建一个真正的 DOM 树，插到文档当中当状态变更的时候，重新构造一棵新的对象树。然后用新的树和旧的树进行比较，记录两棵树差异把 2 所记录的差异应用到步骤 1 所构建的真正的 DOM 树上，视图就更新了。

5. react diff 原理

    * 把树形结构按照层级分解，只比较同级元素。
    * 给列表结构的每个单元添加唯一的 key 属性，方便比较。
    * React 只会匹配相同 class 的 component（这里面的 class 指的是组件的名字）
    * 合并操作，调用 component 的 setState 方法的时候, React 将其标记为 dirty.到每一个事件循环结束, React 检查所有标记 dirty 的 component 重新绘制.
    * 选择性子树渲染。开发人员可以重写 shouldComponentUpdate 提高 diff 的性能。


6. React 中 refs 的作用是什么？

    Refs 是 React 提供给我们的安全访问 DOM 元素或者某个组件实例的句柄。我们可以为元素添加 ref 属性然后在回调函数中接受该元素在 DOM 树中的句柄，该值会作为回调函数的第一个参数返回

7. 展示组件(Presentational component)和容器组件(Container component)之间有何不同
    * 展示组件关心组件看起来是什么。展示专门通过 props 接受数据和回调，并且几乎不会有自身的状态，但当展示组件拥有自身的状态时，通常也只关心 UI 状态而不是数据的状态。
    * 容器组件则更关心组件是如何运作的。容器组件会为展示组件或者其它容器组件提供数据和行为(behavior)，它们会调用 Flux actions，并将其作为回调提供给展示组件。容器组件经常是有状态的，因为它们是(其它组件的)数据源。

8. 类组件(Class component)和函数式组件(Functional component)之间有何不同

    * 类组件不仅允许你使用更多额外的功能，如组件自身的状态和生命周期钩子，也能使组件直接访问 store 并维持状态
    * 当组件仅是接收 props，并将组件自身渲染到页面时，该组件就是一个 '无状态组件(stateless component)'，可以使用一个纯函数来创建这样的组件。这种组件也被称为哑组件(dumb components)或展示组件

9. (组件的)状态(state)和属性(props)之间有何不同

    * State 是一种数据结构，用于组件挂载时所需数据的默认值。State 可能会随着时间的推移而发生突变，但多数时候是作为用户事件行为的结果。
    * Props(properties 的简写)则是组件的配置。props 由父组件传递给子组件，并且就子组件而言，props 是不可变的(immutable)。组件不能改变自身的 props，但是可以把其子组件的 props 放在一起(统一管理)。Props 也不仅仅是数据--回调函数也可以通过 props 传递。

10. 何为受控组件(controlled component)

    在 HTML 中，类似 `<input>`, `<textarea>` 和 `<select>` 这样的表单元素会维护自身的状态，并基于用户的输入来更新。当用户提交表单时，前面提到的元素的值将随表单一起被发送。但在 React 中会有些不同，包含表单元素的组件将会在 state 中追踪输入的值，并且每次调用回调函数时，如 onChange 会更新 state，重新渲染组件。一个输入表单元素，它的值通过 React 的这种方式来控制，这样的元素就被称为"受控元素"。

11. 何为高阶组件(higher order component)

    高阶组件是一个以组件为参数并返回一个新组件的函数。HOC 运行你重用代码、逻辑和引导抽象。最常见的可能是 Redux 的 connect 函数。除了简单分享工具库和简单的组合，HOC 最好的方式是共享 React 组件之间的行为。如果你发现你在不同的地方写了大量代码来做同一件事时，就应该考虑将代码重构为可重用的 HOC。

12. 为什么建议传递给 setState 的参数是一个 callback 而不是一个对象

    因为 this.props 和 this.state 的更新可能是异步的，不能依赖它们的值去计算下一个 state。

13. 除了在构造函数中绑定 this，还有其它方式吗

    你可以使用属性初始值设定项(property initializers)来正确绑定回调，create-react-app 也是默认支持的。在回调中你可以使用箭头函数，但问题是每次组件渲染时都会创建一个新的回调。

14. (在构造函数中)调用 super(props) 的目的是什么

    在 super() 被调用之前，子类是不能使用 this 的，在 ES2015 中，子类必须在 constructor 中调用 super()。传递 props 给 super() 的原因则是便于(在子类中)能在 constructor 访问 this.props。

15. 应该在 React 组件的何处发起 Ajax 请求

    在 React 组件中，应该在 componentDidMount 中发起网络请求。这个方法会在组件第一次“挂载”(被添加到 DOM)时执行，在组件的生命周期中仅会执行一次。更重要的是，你不能保证在组件挂载之前 Ajax 请求已经完成，如果是这样，也就意味着你将尝试在一个未挂载的组件上调用 setState，这将不起作用。在 componentDidMount 中发起网络请求将保证这有一个组件可以更新了。

16. redux的理解

    * redux 是一个应用数据流框架，主要是解决了组件间状态共享的问题，原理是集中式管理，主要有三个核心方法，action，store，reducer，工作流程是 view 调用 store 的 dispatch 接收 action 传入 store，reducer 进行 state 操作，view 通过 store 提供的 getState 获取最新的数据。在 redux 中只能定义一个可更新状态的 store，redux 把 store 和 Dispatcher 合并,结构更加简单清晰
    * 新增 state,对状态的管理更加明确，通过 redux，流程更加规范了，减少手动编码量，提高了编码效率，同时缺点时当数据更新时有时候组件不需要，但是也要重新绘制，有些影响效率。一般情况下，我们在构建多交互，多数据流的复杂项目应用时才会使用它们

17. redux 有什么缺点

    * 一个组件所需要的数据，必须由父组件传过来。
    * 当一个组件相关数据更新时，即使父组件不需要用到这个组件，父组件还是会重新 render，可能会有效率影响，或者需要写复杂的 shouldComponentUpdate 进行判断。

18. setState

    A. 
    * setState不会立刻改变React组件中state的值
    * setState通过引发一次组件的更新过程来引发重新绘制
    * 重绘指的就是引起React的更新生命周期函数4个函数：
        * shouldComponentUpdate（被调用时this.state没有更新；如果返回了false，生命周期被中断，虽然不调用之后的函数了，但是state仍然会被更新）
        * componentWillUpdate（被调用时this.state没有更新）
        * render（被调用时this.state得到更新）
        * componentDidUpdate
    * 多次setState函数调用产生的效果会合并。如果每次调用都引发一次生命周期更新，那性能就会消耗很大了。

    B. 通过this.state直接修改React中state 值，但不会引起重新渲染。

    C. 在React中，如果是由React引发的事件处理（比如：onClick引发的事件处理），调用setState不会同步更新this.state，除此之外的setState调用会同步执行this.setState。 “除此之外”指的是：绕过React通过addEventListener直接添加的事件处理函数和setTimeout/setInterval产生的异步调用。

    D. 可以传两个参数，第一个参数可以以传统方式传递对象，更改state，第二个参数为回调函数，返回更改后的state；第一个参数也可以传一个函数，函数有两个参数，一个是当前的state，一个是当前props，以函数形式可以达到setState同步更新。但是传统模式和函数模式不能共同使用，传统模式会覆盖掉函数模式修改的state。

