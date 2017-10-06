# scrollIntoView-and-scrollIntoView

Hello~亲爱的观众老爷们大家好！国庆中秋长假快放完了，是时候收拾心（ti）情（zhong）好好学习与工作啦。这次为大家带来的是两个好用 API 的介绍，其实也是偷懒神器。

根据 [MDN](https://developer.mozilla.org/zh-CN/docs/Web/API/Element/scrollIntoView)的描述，`Element.scrollIntoView()`方法让当前的元素滚动到浏览器窗口的可视区域内。

而`Element.scrollIntoViewIfNeeded（）`方法也是用来将不在浏览器窗口的可见区域内的元素滚动到浏览器窗口的可见区域。但如果该元素已经在浏览器窗口的可见区域内，则不会发生滚动。此方法是标准的Element.scrollIntoView()方法的专有变体。

因而再有什么回到顶部、去到置顶位置和键盘弹出挡住输入框之类的需求，都可以简单解决了。

然而，面对好用的 API，前端们第一个反映都是，看兼容性！

先看`scrollIntoView`的：

![](https://user-gold-cdn.xitu.io/2017/10/6/fe81e231a974d45ac878b2c0c63ecb5d)

看到一片黄黄绿绿的，基本可以安心，不支持的只是某个属性的取值而已，下面会有介绍~

之后看看`scrollIntoViewIfNeeded`：

![](https://user-gold-cdn.xitu.io/2017/10/6/0f1344a4895d70a52954ed92f58ffcb9)

`IE`和`FireFox`全红，如果`PC`端想用的话，基本只能内部项目了，略为可惜。但移动端还是绿悠悠的，基本都OK，可以安心使用~

由于本文是介绍向~因而每个属性我都写了点小`demo`，点进去就可以体验一下哦！

## scrollIntoView

先介绍`scrollIntoView`,使用起来其实很简单，获取某个元素后，直接调用`scrollIntoViewIfNeeded()`即可，简单的`demo`[点这就好](https://ljf0113.github.io/scrollIntoView-and-scrollIntoView/scrollIntoView-1)，点一下侧边的小绿块，页面就会滚上去。`demo`代码大概长这样：

    <body>
    	<div class="chunk"></div>
    	<div class="btn">click</div>
    	<script>
        const btn = document.querySelector('.btn');
        const test = document.querySelector('.chunk');
        btn.addEventListener('click', function() {
          test.scrollIntoView();
        })
    	</script>
    </body>
    
是不是很简单~不过可能有同学就有疑问了，这不就和锚点定位一样吗？感觉毫无意义啊！先别急，当你调用`scrollIntoView`的时候，其实是可以传参数进去的。`scrollIntoView`只接受一个参数，但接受两种类型的参数，分别是`Boolean`型参数和`Object`型参数。

先说`Boolean`型参数，顾名思义，参数可以使`true`和`false`。如果为`true`，元素的顶端将和其所在滚动区的可视区域的顶端对齐。若为`false`，元素的底端将和其所在滚动区的可视区域的底端对齐。简单的例子[可以点这里](https://ljf0113.github.io/scrollIntoView-and-scrollIntoView/scrollIntoView-2)。主要代码如下：

    <body>
    	<div class="chunk"></div>
    	<div class="btn-top">up</div>
    	<div class="btn-bottom">down</div>
    	<script>
        const up = document.querySelector('.btn-top');
        const down = document.querySelector('.btn-bottom');
        const test = document.querySelector('.chunk');
        up.addEventListener('click', function() {
          test.scrollIntoView(true);
        });
        down.addEventListener('click', function() {
          test.scrollIntoView(false);
        });
    	</script>
    </body>

如你所见到的，当传入参数为分别为`true`与`false`时，当点击右侧的按钮时，红色的`div`会贴近可视区域的顶部或底部。

之后是`Object`型参数，这个对象有两个选项，也就是对象里面的`key`。`block`与之前的`Boolean`型参数一致，不过值不再是`true`和`false`，是更语义化的`start`和`end`。

另一个选项是`behavior`,MDN上给出三个可取的值，分别是`auto`、`instant`与`smooth`。这个选项决定页面是如何滚动的，实测`auto`与`instant`都是瞬间跳到相应的位置，查阅`W3C`后发现了这么一句:"The instant value of scroll-behavior was renamed to auto."。因而基本可以确定两者表现是一致的。而`smooth`就是有动画的过程，可惜的是之前提及兼容性时说过，黄色其实不支持某个属性，就是不支持`behavior`取值为`smooth`。而且，实测了IE及移动端的UC浏览器后发现，它们根本就不支持`Object`型参数，因而在调用`scrollIntoView({...})`时，只有默认的结果，即`scrollIntoView(true)`。简单的例子[看这里](https://ljf0113.github.io/scrollIntoView-and-scrollIntoView/scrollIntoView-3)，如果想体验`smooth`的效果，需要使用Chrome或者Firefox哦！主要代码如下：

    <body>
    	<div class="chunk"></div>
    	<div class="btn-top">up</div>
    	<div class="btn-bottom">down</div>
    	<script>
        const up = document.querySelector('.btn-top');
        const down = document.querySelector('.btn-bottom');
        const test = document.querySelector('.chunk');
        up.addEventListener('click', function() {
          test.scrollIntoView({
            block: 'start',
            behavior: 'smooth'
          });
        });
        down.addEventListener('click', function() {
          test.scrollIntoView({
            block: 'end',
            behavior: 'smooth'
          });
        });
    	</script>
    </body>

## scrollIntoViewIfNeeded 
介绍完`scrollIntoView`，是时候介绍一下它的变体`scrollIntoViewIfNeeded`了。两者主要区别有两个。首先是`scrollIntoViewIfNeeded`是比较懒散的，如果元素在可视区域，那么调用它的时候，页面是不会发生滚动的。其次是`scrollIntoViewIfNeeded`只有`Boolean`型参数，也就是说，都是瞬间滚动，没有动画的可能了。

`scrollIntoViewIfNeeded`可以接受一个`Boolean`型参数，和`scrollIntoView`不同，`true`为默认值，但不是滚动到顶部，而是让元素在可视区域中居中对齐；`false`时元素可能顶部或底部对齐，视乎元素靠哪边更近。简单的例子[可以点这里](https://ljf0113.github.io/scrollIntoView-and-scrollIntoView/scrollIntoViewIfNeeded)。大致代码如下：

    <body>
    	<div class="chunk"></div>
    	<div class="scrollIntoView">scrollIntoView top</div>
    	<div class="scrollIntoViewIfNeeded-top">scrollIntoViewIfNeeded top</div>
    	<div class="scrollIntoViewIfNeeded-bottom">scrollIntoViewIfNeeded botom</div>
    	<script>
        const scrollIntoView = document.querySelector('.scrollIntoView');
        const scrollIntoViewIfNeededTop = document.querySelector('.scrollIntoViewIfNeeded-top');
        const scrollIntoViewIfNeededBottom = document.querySelector('.scrollIntoViewIfNeeded-bottom');
        const test = document.querySelector('.chunk');
        scrollIntoView.addEventListener('click', function() {
          test.scrollIntoView(true);
        });
        scrollIntoViewIfNeededTop.addEventListener('click', function() {
          test.scrollIntoViewIfNeeded(true);
        });
        scrollIntoViewIfNeededBottom.addEventListener('click', function() {
          test.scrollIntoViewIfNeeded(false);
        });
    	</script>
    </body>
    
如文档所说，当红色的`div`完全在可视区域的情况下，调用`scrollIntoView()`是会发生滚动，而调用`scrollIntoViewIfNeeded()`则不会。而我实践后发现了一些文档没有的细节。当元素处于可视区域，但不是全部可见的情况下，调用`scrollIntoViewIfNeeded()`时，无论参数是`true`还是`false`，都会发生滚动，而且效果是滚动到元素与可视区域顶部或底部对齐，视乎元素离哪端更近。这个大家需要注意一下~

## 小结

其实这个API并不是必须的，有很多其他方法可以达到它的效果。然而在条件许可的情况下，使用一下是能节省下好多的`JS`代码或是一堆锚点，还是很爽的。

感谢各位看官大人看到这里~希望本文对你有所帮助，记得使用一下这两个API哦！
