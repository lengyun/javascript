# 历史状态管理

通过hashchange事件，可以知道URL的参数什么时候发生了变化，即什么时候该有所反映。而通过状态管理API，能够在不加载新页面的情况下改变浏览器的URL。为此，需要使用 **history.pushState()**方法，该方法可以接受三个参数：状态对象、新状态的标题和可选的相对URL

```js
history.pushState({name:"Nicholas"}, "Nicholas' page", "nicholas.html");
```

执行pushState()方法后，新的状态信息就会被加入历史状态栈，而浏览器地址栏也会变成新的相对URL。但是，浏览器并不会真的向服务器发送请求，即使状态改变后查询location.href也会返回与地址栏中相同的地址。第一个参数则应该尽可能提供初始化页面状态所需的各种信息。第二个参数只传一个空字符串。

执行 pushState()会创建新的历史状态，所以你会发现“后退”按钮也能使用了。按下“后退”按钮，就会触发window对象的popstate事件。popstate事件的事件对象有一个state属性，这个属性就包含着当初以第一个参数传递给pushState()的状态对象。

```js
EventUtil.addHandler(window, "popstate", function(event){
    var state = event.state;
    if (state){ //第一个页面加载时 state 为空
    	processState(state);
    }
});
```

要更新当前状态，可以调用 replaceState()，传入的参数与 pushState()的前两个参数相同。
调用这个方法不会在历史状态栈中创建新状态，只会重写当前状态。

```js
history.replaceState({name:"Greg"}, "Greg's page");
```

