# 虚拟DOM和DON diff


## 虚拟DOM


### 虚拟DOM优点

**减少DOM操作**

  虚拟DOM可以将多次操作合并为一次操作，例如当我们添加1000个节点时将一个节点一个节点添加的操作转变为一次性操作
  
  虚拟DOM借助DOM diff可以将多余的操作省掉，添加的1000个节点其实只有10个时新增的
  
**跨平台**
  
  虚拟DOM不仅可以变成DOM，还可以变成小程序、ios应用、安卓应用，因为虚拟DOM本质上只是一个JS对象。


### 虚拟DOM代码

```
React

const vNode = {
  key: null,
  props: {
    children: [ //子元素们、
      { type: 'span',...},
      { type: 'span',...}
   ],
   className: "red"//标签上的属性
   onClick: () => {} //事件
}，
ref: null,
type:"div",//标签名 or 组件名
...
}
      
```

```
Vue

const vNode = {
  tag: "div",// 标签名 or 组件名
  data: {
    class: "red", // 标签上的属性
    on: {
      click: () => {} //事件
    }
  },
  children: [ // 子元素们
     { tag: "span", ...},
     { tag: "span", ...},
   ],
   ...
}
```

### 如何创建虚拟DOM

**React.createElement**

```
createElement('div',{className:'red',onClick: () => {}},[
   createElement('span',{},'span1'),
   createElement('span',{},'span2'),
  ]
)
```

**Vue (只能在render函数里得到h)**

```
h('div',{
  class: 'red',
  on: {
    click: () => { }
   },
 },[h('span',{},'span1'),h('span',{},'span2'])
```

**使用JSX简化创建虚拟DOM**

```
React JSX

<div className="red" onClick="{()=>{}}">
    <span>span1</span>
    <span>span2</span>
</div>
```

```
Vue Template

h('div',{
  class: 'red',
  on: {
     click: () => { }
   },
 },[h('span',{},'span1'),h('span',{},'span2'])
```

**目前创建虚拟DOM的方法**

```
React

<div className="red" onClick={fn}>
   <span>span1</span>
   <span>span2</span>
</div>
通过babel转为createElement形式
```

```
Vue Template

<div class="red" @click="fn">
   <span>span1</span>
   <span>span2</span>
</div>

通过vue-loader 转为h形式
```

### 总结

**虚拟DOM是什么**
 
 一个能代表DOM树的对象，通常含有标签名、标签上的属性、事件监听和子元素们，以及其他属性
 
**虚拟DOM有什么优点**

  能减少不必要的DOM操作；</br>
  能跨平台渲染；

**虚拟DOM有什么缺点**

 需要额外的创建函数，如createElement或h,但可以通过JSX来简化成XML写法

## DOM diff

虚拟DOM的对比算法

### 什么是DOM diff

 * DOM diff是一个函数，我们称之为patch;
 * patches = patch(oldVNode,newVNode);
 * patches 就是要运行的DOM操作，以代码为例
```
[
  {type: 'INSERT', vNode: ... },
  {type: 'TEXT', vNode: ...},
  {type: 'PROPS', propsPatch: [...]}
]
```

### DOM diff的大概逻辑

**Tree diff**

* 将新旧两棵树逐层对比，找出哪些节点需要更新；
* 如果节点是组件就看Component diff;
* 如果节点是标签就看Element diff;

**Component diff**

* 如果节点是组件，就先看组件类型；
* 类型不同直接替换(删除旧的)；
* 类型相同则只更新属性；
* 然后深入组件做Tree diff(递归)；

**Element diff**

* 如果节点是原生标签，则看标签名；
* 标签名不同直接替换，相同则只更新属性；
* 然后进入标签后代做Tree diff (递归)









