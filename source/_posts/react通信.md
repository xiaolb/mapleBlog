---
title: react通信
date: 2018-09-12 17:58:44
tags:
---


## 通信的几种方式

一般来说，react里边有几种常用的方式：父到子，子到父，兄弟组件之间的通信。

===== 父向子通信 =====

- 直接标签中插入参数即可

```
let _number = this.state.number
<Child number={_number} />

// 在子组件中就能直接通过this.props.number来调用父组件传的值
```

需要注意，_number 可以是普通参数、this.state.xxx 参数、也可以是this.props.xxx参数，其中this.state.xxx参数若发生改变，会导致 Child 重新渲染

===== 子向父通信 =====

- 需要从 Parent 处接一个绑定了父组件的函数，然后在 Child 内部调用修改父的相关参数，达到效果

```
// 父组件在调用子组件的时候传递一个函数
turn(option) {
	this.setState({option:option})
}
let dotsNode1 = <SliderDots turn={this.turn.bind(this)}/>

// 子组件中调用this.props.turn(option)若父组件中存在turn则会将option传递给父组件，从而会调用父组件中的turn函数。
handleDotClick(count) {
	let option = count-this.props.nowPage;
	this.props.turn(option);
}

render() {
	return (
	    <span 
	    	key={'dot'+index}
	    	onClick={this.handleDotClick.bind(this,index)}>
	    </span>
	)
}
```

同样，可以修改父组件的 this.state.xxx（ this.setState() 触发渲染），也可以修改this.xxx等值


===== 兄弟组件之间通信 =====

- 需要从 Parent 处接一个绑定了父组件的函数，然后在 ChildOne 内部调用修改父的相关参数，再将参数传递给childTwo，达到效果。

```
// 在上述子传父的基础上，有一个dotNode2，在dotsNode2中绑定option，就能用了。
let dotsNode2 = <SliderDots option = {this.state.option}/>
```
同样，可以修改父组件的 this.state.xxx（ this.setState() 触发渲染），也可以修改this.xxx等值.  

实质上，改变其中一个兄弟组件的值，传递给父元素，再setState一下，组件就会重新渲染，在另外一个兄弟组件中的值就会发生变化，达到了兄弟组件之间值传递的效果。

具体的可以看看 [轮播图](https://github.com/xiaolb/react-swiper "react实现轮播") 和 [评论](https://github.com/xiaolb/comment) 这两个小栗子。

===== 结论 =====

组件之间的通信说着比较容易，但是在实际上用的时候还是有点压力的，多用。

本文只初步的说明了组件之间的通信。针对于不复杂的程序还是够了，但是对于比较复杂的，通信需要用到redux。