## 前端应用框架模式(mv*)

> 最早接触架构模式，还是在从事java的时候，那时候流行ssh框架的mvc模式，沧海桑田，转眼间已从事前端多年，架构模式也从mvc发展到了多样化。这也归功于前端技术的流行和复杂化，一起看看这些模式吧，起码我们要清楚，我们现在用的技术，到底是什么模式。

mv*模式就是为了解耦数据模型和UI层。

从历史的角度来看，mvc,mvp, mvvm是一种进化关系，他们的规模及模式实现的方式不同，各有优缺点，当然了既然是一种进化，mvvm被认为是前端领域最好的模式。

### MVC

> Model-View-Controller

- Model 进行逻辑业务处理
- View 应用数据的可视化显示
- Controller 协调和管理应用程序中Model和View之间的逻辑

> 用户对视图层的操作逻辑不是在view中去处理，而是由Controller层处理，Controller对这些操作中的数据经过应用逻辑的操作之后，然后调用Model层的接口，将数据交给Model层，Model执行与业务逻辑相关的操作，并更新数据。Model和View通过观察者模式联系在一起，即View是Model的观察者，当Model数据变动之后，通知View层进行数据更新.

### MVP

> Model-View-Presenter

- Model 还是业务逻辑的处理
- View 还是应用程序的可视化表示，但是它对Model层，完全无知，比起mvc中view，更轻了
- Presenter 这层比较重，它不仅调用Model的接口，也要调用View的接口，而且需要作为观察者获得Model的数据更新

> MVP中M与V之间的依赖关系被消除了，他们之间的观察者模式依赖转移到M和P层中，这样P层必须通过一定的机制通知V层进行数据更新，所以MVP模式中V层提供了供P层调用的接口，P层作为观察者活的了数据变化后将调用V层的接口将变化反映到V层中。

### MVVM

> Model-View-ViewModel

- Model 还是数据逻辑(也没有m和v的依赖) 
- View 还是应用程序的可视化表示
- ViewModel 数据转换器，它将Model信息转为View信息，还将命令从View传递到Model.

> M层数据的变化不是通过观察者模式通知到V层的，也不是通过VM层调用V层接口将数据传递给V层的(这意味着用户代码不需要手动更新V层)， 二十通过在VM层实现了一个特殊的binder,将数据从M层直接绑定到V层。这样ViewModel层了解Model层，View层了解ViewModel层。

深层理解一下ViewModel:

- ViewModel为View提供了一个访问Model的桥梁，但是View不是直接访问Model层。ViewModel为View提供了Model特定于View的子集，比如状态和逻辑信息，而且无需向View暴露整个Model。
- View和ViewModel通过数据绑定和事件可以进行通信，因为ViewModel可以访问Model层，所以ViewModel为了数据绑定要暴露Model中的部分属性。

> MVVM俗称为双向绑定，可以简单而不恰当地理解为一个模板引擎，但是会根据数据变更实时渲染。


### 前端框架对应的mv** 模式

- angularjs
- emberjs
- vue
- react
- backbone


