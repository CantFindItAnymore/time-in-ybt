### 1. 概述

我好笨，学不会hooks :sob:

再来一次

react分为类组件(有状态组件class)和函数式组件(无状态组件function)，

react推崇无状态组件的写法，所以提出了hooks。

使用hooks后，我们可以

- 在无状态组件中使用状态，
- 不再需要生命周期函数
- 不再需要bind(this)

以后，我们要渐渐放弃class的写法了，function天下第一(开心，又可以掉头发了 :slightly_smiling_face:)

然后，我们先列出几种常用的hooks以及它的作用

- `useState`：在函数式组件中使用状态（函数中竟然有状态，这就离谱~）
- `useEffect`：在函数式组件中使用生命周期钩子，一个抵三个(`componentDidMount`，`componentDidUpdate`和`componentWillUnmount`)
- `useRef`
- `useMemo`
- `useCallback`
- `useReducer`
- `useContext`：在函数式组件中提供上下文环境
- `自定义hook`



举个例子👇，下面两种写法效果一致

```javascript
/*
* 类组件
*/
class Example extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      count: 0
    };
  }

  render() {
    return (
      <div>
        <p>You clicked {this.state.count} times</p>
        <button onClick={() => this.setState({ count: this.state.count + 1 })}>
          Click me
        </button>
      </div>
    );
  }
}
```

```javascript
/*
* 使用hooks的无状态组件
*/
import { useState } from 'react';

function Example() {
  const [count, setCount] = useState(0);

  return (
    <div>
      <p>You clicked {count} times</p>
      <button onClick={() => setCount(count + 1)}>
        Click me
      </button>
    </div>
  );
}
```

​	其实，hooks本质上是一些特殊的函数，他让我们能在函数式组件中使用一些特殊的功能。





接着我们来详细说一下这几个hook

### 2. `useState`

在函数组件里面使用 class的`setState`

```javascript
/*
* useState
*/

import React, { useState } from 'react'

const LikeButton = () => {
  // useState用于在函数组件中添加和管理状态
  // useStete之间互不相干，但一个useState中的多个state必须同步更新 ↓

  // 1.
  // const [ like, setLike ] = useState(0)
  // const [ on, setOn ] = useState('on')

  // 2.
  const [ obj, setObj ] = useState({like: 0, on: true})

  return (
    <>
      <button onClick={
        () => { setObj({ like: obj.like+1, on: obj.on }) }
      }>
        { obj.like } 赞
      </button>

      <button onClick = {
        () => { setObj({ like: obj.like, on: !obj.on }) }
      }>
        状态：{ obj.on ? 'on' : 'off' }
      </button>
    </>
  )
}

export default LikeButton
```



### 3. `useEffect`

在函数组件中使用class的生命周期函数，并且还是这几个生命周期函数的集合

```javascript
/*
* useEffect
*/

import React, { useState, useEffect } from 'react'

const MouseChange = () => {
  // ----------------------------------
  // useEffect
  // 可以理解为这样一个生命周期：( 组件加载完成 || 组件更新完成 || 组件即将卸载 )时

  // 1. 产生某些副作用 ↓
  useEffect(() => {
    console.log('useEffect -- do something')
  })

  // 2. 消除某些副作用 ↓
  const [ position, setPosition ] = useState({x: 0, y: 0})
  useEffect(() => {
    // 下面这种注释掉的写法--每次组件更新时都会添加一个监听事件，会大量重复(内存泄漏，页面卡死)！so，我们要清除它！
    // document.addEventListener('click', (e) => {
    //   console.log('有几个我？')
    //   setPosition({ x: e.clientX, y: e.clientY })
    // })

    // 下面这种写法这样理解 ↓
    // 这其实是一个发布订阅模式，在组件加载完成或更新时订阅监听器，在组件卸载时清除监听器。
    const mouseUpdated = (e) => {
      console.log('订阅监听器')
      setPosition({ x: e.clientX, y: e.clientY })
    }

    document.addEventListener('click', mouseUpdated)
    // 清除副作用函数
    // 在组件卸载时执行(可以理解为class中触发render时) ↓
    return () => {
      console.log('移除监听器')
      document.removeEventListener('click', mouseUpdated)
    }
  })

  return (
    <p>
      X: { position.x }<br />
      Y: { position.y }
    </p>
  )
}

export default MouseChange
```



