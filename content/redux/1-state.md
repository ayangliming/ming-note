
现在来实现表单提交事件的相应，添加 `handleSubmit` 如....

```js
<form className="comment-form" onSubmit={this.handleSubmit}>
          <input type="text" className="input" value={input} onChange={e=>this.setState({input:e.target.value})}/>
          <button type="submit" className="submit-btn">提交</button>
</form>
```

名词注释：
submit: 提交
handle：处理
form：表单

handleSubmit=()=>{
	e.preventDefault()
	console.log('from submitting...')

}

由于使用了Es6 的胖箭头函数语法，用以使用中,就不需要使用bind(this)了

`e.preventDefault()`的作用是

>阻止页面跳转

### 拿到input的值

首先添加 ref 如下：

```js
<input type="text" className="input" ref={value=>this.comment = value}/>
```
上面.ref的意思是"指针"，后面大括号中的内容，也就是

```
value => this.comment = value
```
是一个胖箭头函数。其中value 指当代 input 这个节点对应的 Object(对象)然后，

```
this.comment = value
```

就是把 input 对象， 赋值给一个成员变量，目的是在整个 class 之内。可以随处访问


这样如果想在 handleSubmit 函数中拿到用户输入的文本，就

```js
handleSubmit= ()=>{
	console.log(this.comment.value)
}
```

到这里，如何从 form 的输入框中拿到字符窜，这个技巧我们就掌握了，同时
这也是最简单的一种方式，另外，还可以通过"受控组件"的形式来取值，那个
比较麻烦我们暂时不学

### 修改 state

先来讨论 render() 的本性：

>一旦 state 变，render() 自动被重新执行
所以，界面上由两条评论变成三条评论我们根本就不需要
改变html，直接setState即可。

### 清空 input
最后，我们还需要在提交结束后清空 iuput 里面的字符串
handleSUbmit 中添加

```js
this.comment.value=""
```
但是如果一个 form 的 input 比较多，那么下面的方式可能更好

```js
handleSubmit=()=>{
	this.myForm.reset()
}
```


```
<form className="comment-form" onSubmit={this.handleSubmit} 
 ref={value=> this.myForm=value}></form>
```
