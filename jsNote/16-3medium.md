# 媒体元素

 HTML5 新增了两个与媒体相关的标签，让开发人员不必依赖任何插件就能在网页中嵌入跨浏览器的音频和视频内容。这两个标签就是<audio>和<video>。

这两个标签除了能让开发人员方便地嵌入媒体文件之外，都提供了用于实现常用功能的 JavaScript
API，允许为媒体创建自定义的控件。这两个元素的用法如下。

```html
<!-- 嵌入视频 -->
<video src="conference.mpg" id="myVideo">Video player not available.</video>
<!-- 嵌入音频 -->
<audio src="song.mp3" id="myAudio">Audio player not available.</audio>
```

使用这两个元素时，至少要在标签中包含 **src 属性**，指向要加载的媒体文件。还可以设置 width和 height 属性以指定视频播放器的大小，而为 **poster 属性**指定图像的 URI 可以在加载视频内容期间显示一幅图像。另外，如果标签中有 **controls 属性**，则意味着浏览器应该显示 UI 控件，以便用户直接操作媒体。位于开始和结束标签之间的任何内容都将作为后备内容，在浏览器不支持这两个媒体元素的情况下显示。

因为并非所有浏览器都支持所有媒体格式，所以可以指定多个不同的媒体来源。为此，不用在标签
中指定 src 属性，而是要像下面这样使用一或多个<source>元素。

```html
<!-- 嵌入视频 -->
<video id="myVideo">
    <source src="conference.webm" type="video/webm; codecs='vp8, vorbis'">
    <source src="conference.ogv" type="video/ogg; codecs='theora, vorbis'">
    <source src="conference.mpg">
    Video player not available.
</video>
<!-- 嵌入音频 -->
<audio id="myAudio">
    <source src="song.ogg" type="audio/ogg">
    <source src="song.mp3" type="audio/mpeg">
    Audio player not available.
</audio>
```

## 属性

<video>和<audio>元素都提供了完善的 JavaScript 接口。下表列出了这两个元素共有的属性，通过这些属性可以知道媒体的当前状态。

| 属性 | 数据类型 | 说明 |
| :--- | :------- | :--- |
| autoplay | 布尔值 | 取得或设置autoplay标志 |
| buffered | 时间范围 | 表示已下载的缓冲的时间范围的对象 |
| bufferedBytes | 字节范围 | 表示已下载的缓冲的字节范围的对象 |
| bufferingRate  | 整数  | 下载过程中每秒钟平均接收到的位数 |
| bufferingThrottled  | 布尔值  | 表示浏览器是否对缓冲进行了节流 |
| controls  | 布尔值  | 取得或设置controls属性，用于显示或隐藏浏览器内置的控件 |
| currentLoop  | 整数  | 媒体文件已经循环的次数 |
| currentSrc  | 字符串 |  当前播放的媒体文件的URL |
| currentTime  | 浮点数  | 已经播放的秒数 |
| defaultPlaybackRate  | 浮点数 |  取得或设置默认的播放速度。默认值为1.0秒 |
| duration  | 浮点数  | 媒体的总播放时间（秒数） |
| ended  | 布尔值 |  表示媒体文件是否播放完成 |
| loop  | 布尔值 |  取得或设置媒体文件在播放完成后是否再从头开始播放 |
| muted  | 布尔值  | 取得或设置媒体文件是否静音 |
| networkState  | 整数  | 表示当前媒体的网络连接状态： 0表示空， 1表示正在加载， 2表示正在加载元数据， 3表示已经加载了第一帧， 4表示加载完成 |
| paused  | 布尔值  | 表示播放器是否暂停 |
| playbackRate  | 浮点数  | 取得或设置当前的播放速度。用户可以改变这个值，让媒体播放速度变快或变慢，这与defaultPlaybackRate只能由开发人员修改的defaultPlaybackRate不同 |
| played  | 时间范围  | 到目前为止已经播放的时间范围 |
| readyState  | 整数  | 表示媒体是否已经就绪（可以播放了）。 0表示数据不可用， 1表示可以显示当前帧， 2表示可以开始播放， 3表示媒体可以从头到尾播放 |
| seekable | 时间范围  | 可以搜索的时间范围 |
| seeking  | 布尔值  | 表示播放器是否正移动到媒体文件中的新位置 |
| src  | 字符串  | 媒体文件的来源。任何时候都可以重写这个属性 |
| start  | 浮点数  | 取得或设置媒体文件中开始播放的位置，以秒表示 |
| totalBytes  | 整数  | 当前资源所需的总字节数 |
| videoHeight  | 整数 |  返回视频（不一定是元素）的高度。只适用于<video> |
| videoWidth  | 整数 |  返回视频（不一定是元素）的宽度。只适用于<video> |
| volume  | 浮点数 |  取得或设置当前音量，值为0.0到1.0 |

## 事件

除了大量属性之外，这两个媒体元素还可以触发很多事件。这些事件监控着不同的属性的变化，这些变化可能是媒体播放的结果，也可能是用户操作播放器的结果。下表列出了媒体元素相关的事件。

| 事件 | 触发时机 |
| ---- | -------- |
|abort |下载中断|
|canplay |可以播放时； readyState值为2|
|canplaythrough| 播放可继续，而且应该不会中断； readyState值为3|
|canshowcurrentframe |当前帧已经下载完成； readyState值为1|
|dataunavailable |因为没有数据而不能播放； readyState值为0|
|durationchange |duration属性的值改变|
|emptied |网络连接关闭|
|empty |发生错误阻止了媒体下载|
|ended |媒体已播放到末尾，播放停止|
|error |下载期间发生网络错误|
|load |所有媒体已加载完成。这个事件可能会被废弃，建议使用canplaythrough|
|loadeddata |媒体的第一帧已加载完成|
|loadedmetadata |媒体的元数据已加载完成|
|loadstart |下载已开始|
|pause |播放已暂停|
|play |媒体已接收到指令开始播放|
|playing |媒体已实际开始播放|
|progress |正在下载|
|ratechange |播放媒体的速度改变|
|seeked |搜索结束|
|seeking |正移动到新位置|
|stalled |浏览器尝试下载，但未接收到数据|
|timeupdate |currentTime被以不合理或意外的方式更新|
|volumechange |volume属性值或muted属性值已改变|
|waiting |播放暂停，等待下载更多数据|

## 自定义媒体播放器

使用<audio>和<video>元素的**`play()`**和 **`pause()`**方法，可以手工控制媒体文件的播放。组合使用属性、事件和这两个方法，很容易创建一个自定义的媒体播放器：

```html
<div class="mediaplayer">
    <div class="video">
        <video id="player" src="movie.mov" poster="mymovie.jpg"
        	width="300" height="200">
        	Video player not available.
        </video>
    </div>
    <div class="controls">
    	<input type="button" value="Play" id="video-btn">
    	<span id="curtime">0</span>/<span id="duration">0</span>
    </div>
</div>
```

```js
var player = document.getElementById("player"),
    btn = document.getElementById("video-btn"),
    curtime = document.getElementById("curtime"),
    duration = document.getElementById("duration");
//更新播放时间
duration.innerHTML = player.duration;
//为按钮添加事件处理程序
EventUtil.addHandler(btn, "click", function(event){
    if (player.paused){
        player.play();
        btn.value = "Pause";
    } else {
        player.pause();
        btn.value = "Play";
    }
});
//定时更新当前时间
setInterval(function(){
	curtime.innerHTML = player.currentTime;
}, 250);
```

## 检测编解码器的支持情况

并非所有浏览器都支持<video>和<audio>的所有编解码器，而这基本上就意味着你必须提供多个媒体来源。不过，也有一个 JavaScript API 能够检测浏览器是否支持某种格式和编解码器。

这两个媒体元素都有一个 **canPlayType()**方法，该方法接收一种格式/编解码器字符串，返回
"probably"、 "maybe"或""（ 空字符串）。空字符串是假值，因此可以像下面这样在 if 语句中使用
canPlayType()：

```js
if (audio.canPlayType("audio/mpeg")){
    //进一步处理
}
```

而"probably"和"maybe"都是真值，因此在 if 语句的条件测试中可以转换成 true。
如果给 canPlayType()传入了一种 MIME 类型，则返回值很可能是"maybe"或空字符串。这是因
为媒体文件本身只不过是音频或视频的一个容器，而真正决定文件能否播放的还是编码的格式。在同时传入 MIME 类型和编解码器的情况下，可能性就会增加，返回的字符串会变成"probably"。

```js
var audio = document.getElementById("audio-player");
//很可能"maybe"
if (audio.canPlayType("audio/mpeg")){
//进一步处理
}
//可能是"probably"
if (audio.canPlayType("audio/ogg; codecs=\"vorbis\"")){
//进一步处理
}
```

## Audio类型

<audio>元素还有一个原生的 JavaScript 构造函数 Audio，可以在任何时候播放音频。从同为 DOM元素的角度看， Audio 与 Image 很相似，但 Audio 不用像 Image 那样必须插入到文档中。只要创建一个新实例，并传入音频源文件即可。

```js
var audio = new Audio("sound.mp3");
EventUtil.addHandler(audio, "canplaythrough", function(event){
	audio.play();
});
```

创建新的 Audio 实例即可开始下载指定的文件。下载完成后，调用 play()就可以播放音频。