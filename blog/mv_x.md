## 前端应用框架模式(mv*)

> 最早接触架构模式，还是在从事java的时候，那时候流行ssh框架的mvc模式，沧海桑田，转眼间已从事前端多年，架构模式也从mvc发展到了多样化。这也归功于前端技术的流行和复杂化，一起看看这些模式吧，起码我们要清楚，我们现在用的技术，到底是什么模式。

mv*模式就是为了解耦数据模型和UI层。

从历史的角度来看，mvc,mvp, mvvm是一种进化关系，他们的规模及模式实现的方式不同，各有优缺点，当然了既然是一种进化，mvvm被认为是前端领域最好的模式。

### MVC

---

> Model-View-Controller

![mvc](https://yulongge.github.io/images/mvc/mvc.png)

- Model 进行逻辑业务处理
- View 应用数据的可视化显示
- Controller 协调和管理应用程序中Model和View之间的逻辑

> 用户对视图层的操作逻辑不是在view中去处理，而是由Controller层处理，Controller对这些操作中的数据经过应用逻辑的操作之后，然后调用Model层的接口，将数据交给Model层，Model执行与业务逻辑相关的操作，并更新数据。Model和View通过观察者模式联系在一起，即View是Model的观察者，当Model数据变动之后，通知View层进行数据更新.

#### 优点

- 把业务逻辑和展示逻辑分离，模块化程度高。且当应用逻辑需要变更的时候，不需要变更业务逻辑和展示逻辑，只需要Controller换成另外一个Controller就行了（Swappable Controller）
- 观察者模式可以做到多视图同时更新


#### 缺点

- Controller测试困难, 因为视图同步操作是由View自己执行，而View只能在有UI的环境下运行。在没有UI环境下对Controller进行单元测试的时候，应用逻辑正确性是无法验证的：Model更新的时候，无法对View的更新操作进行断言

- View无法组件化。View是强依赖特定的Model的，如果需要把这个View抽出来作为一个另外一个应用程序可复用的组件就困难了。因为不同程序的的Domain Model是不一样的

### MVP

---

> Model-View-Presenter

![mvc](https://yulongge.github.io/images/mvc/mvp.png)

- Model 还是业务逻辑的处理
- View 还是应用程序的可视化表示，但是它对Model层，完全无知，比起mvc中view，更轻了
- Presenter 这层比较重，它不仅调用Model的接口，也要调用View的接口，而且需要作为观察者获得Model的数据更新

> MVP中M与V之间的依赖关系被消除了，他们之间的观察者模式依赖转移到M和P层中，这样P层必须通过一定的机制通知V层进行数据更新，所以MVP模式中V层提供了供P层调用的接口，P层作为观察者活的了数据变化后将调用V层的接口将变化反映到V层中。

#### 优点

- 便于测试,Presenter对View是通过接口进行，在对Presenter进行不依赖UI环境的单元测试的时候。可以通过Mock一个View对象，这个对象只需要实现了View的接口即可
- View 可以进行组件化，因为在mvp中View不依赖Model。

#### 缺点

- Presenter中除了应用逻辑以外，还有大量的View->Model，Model->View的手动同步逻辑，造成Presenter比较笨重，维护起来会比较困难

### MVVM

---

> Model-View-ViewModel

![mvc](https://yulongge.github.io/images/mvc/mvvm.png)

- Model 还是数据逻辑(也没有m和v的依赖) 
- View 还是应用程序的可视化表示
- ViewModel 数据转换器，它将Model信息转为View信息，还将命令从View传递到Model.

> M层数据的变化不是通过观察者模式通知到V层的，也不是通过VM层调用V层接口将数据传递给V层的(这意味着用户代码不需要手动更新V层)， 二十通过在VM层实现了一个特殊的binder,或者是Data-binding engine的东西,将数据从M层直接绑定到V层。这样ViewModel层了解Model层，View层了解ViewModel层。

深层理解一下ViewModel:

- ViewModel为View提供了一个访问Model的桥梁，但是View不是直接访问Model层。ViewModel为View提供了Model特定于View的子集，比如状态和逻辑信息，而且无需向View暴露整个Model。
- View和ViewModel通过数据绑定和事件可以进行通信，因为ViewModel可以访问Model层，所以ViewModel为了数据绑定要暴露Model中的部分属性。

> MVVM俗称为双向绑定，可以简单而不恰当地理解为一个模板引擎，但是会根据数据变更实时渲染。

#### 优点

- 提高可维护性。解决了MVP大量的手动View和Model同步的问题，提供双向绑定机制。提高了代码的可维护性。
- 简化测试。因为同步逻辑是交由Binder做的，View跟着Model同时变更，所以只需要保证Model的正确性，View就正确。大大减少了对View同步更新的测试。

#### 缺点

- 过于简单的图形界面不适用，或说牛刀杀鸡
- 对于大型的图形应用程序，视图状态较多，ViewModel的构建和维护的成本都会比较高
- 数据绑定的声明是指令式地写在View的模版当中的，这些内容是没办法去打断点debug的


### 常见框架对应的mv** 模式

---

- angularjs : 在多次的api重构和改善中，它越来越接近于mvvm模式,但是angular团队称为mvw模式(w for whatever)
- emberjs : mvvm
- vue : 可以说是mvvm的最佳实践，专注于mvvm中的ViewModel,不仅做到了双向绑定，也是一款相对比较轻量级的js库，相比于react更容易上手.
- backbone : mvc
- react 专注view层，不包括数据访问和路由，中心是Component，所以不归属于它们



> 诚然，框架帮我们实现了MV*模式，不代表我们写的代码也是按照MV*模式组织的。因为我们使用了框架，本身就意味着我们认同它的架构和模式，然后调用框架的API，来实现业务层代码。因此即使用户代码中出现类似vm,view等等的命名，也不代表这段代码就是MV*中某一层的实现，而更像是对某一层的实现的一个扩展。而且不同的模式下的用户代码量级确实有时会有显著差距


### 参考

---

- https://github.com/livoras/blog/issues/11
- https://segmentfault.com/a/1190000006145707
- http://www.cnblogs.com/syfwhu/p/4883539.html?utm_source=tuicool&utm_medium=referral
- http://www.cnblogs.com/onepixel/p/6034307.html
- http://www.cnblogs.com/darklx/p/6061081.html
- http://www.cnblogs.com/xishuai/p/mvc-mvp-mvvm-angularjs-knockoutjs-backbonejs-reactjs-emberjs-avalonjs.html
