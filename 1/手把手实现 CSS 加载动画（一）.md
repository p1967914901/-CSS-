# 手把手实现 CSS 加载动画（一）
首先我们来看看最终首先的效果：

![GIF.gif](https://cdn.nlark.com/yuque/0/2022/gif/22793328/1659837927616-3027ba48-98fd-45eb-94ea-6fe4505104c5.gif#clientId=u5249113b-b2de-4&crop=0&crop=0&crop=1&crop=1&from=drop&id=u0b39e12f&margin=%5Bobject%20Object%5D&name=GIF.gif&originHeight=409&originWidth=607&originalType=binary&ratio=1&rotation=0&showTitle=false&size=192210&status=done&style=none&taskId=u67a739aa-0849-4a52-8db3-6c4540e7fc0&title=)

首先我们需要创建三个 `div` 分别表示这三个球以及一个放置容器：

```html
<div class="container">
  <div class="dot dot-1"></div>
  <div class="dot dot-2"></div>
  <div class="dot dot-3"></div>
</div>
```

既然是加载动画，我们不可能让他在角落展示吧，我们当然需要将它水平垂直居中，水平垂直居中的方式有很多，这里就不再赘述了。

```css
.container {
  width: 200px;
  height: 200px;
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
}
```

虽然我们已经水平垂直居中了，但目前还是看不到任何效果。

接下来我们来绘制三个圆，首先一开始三个圆是在容器中间。

```javascript
.dot {
  width: 70px;
  height: 70px;
  border-radius: 50%;
  background-color: #000;
  position: absolute;
  top: 0;
  bottom: 0;
  left: 0;
  right: 0;
  margin: auto;
}
```

现在你已经看到了三个黑乎乎的圆已经重合在容器中间（为了方便观察，我们给容器加了一个背景颜色）。

![image.png](https://cdn.nlark.com/yuque/0/2022/png/22793328/1659846034878-897581d6-5032-49b5-aa0c-cd46ef46b60a.png#clientId=u5249113b-b2de-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=223&id=u8f1b5f74&margin=%5Bobject%20Object%5D&name=image.png&originHeight=279&originWidth=308&originalType=binary&ratio=1&rotation=0&showTitle=false&size=2231&status=done&style=none&taskId=u0b981969-d13f-4b49-9f9e-dc185b88d66&title=&width=246.4)

接下来自然要为三个圆添加不同的颜色：

```javascript
.dot-3 {
  background-color: #f74d75;
}

.dot-2 {
  background-color: #10beae;
}

.dot-1 {
  background-color: #ffe386;
}
```

好了，到现在我们已经完成了初始状态，接下来我们要为它们分别定义动画。

首先先看 `.dot-3`。

```css
.dot-3 {
  background-color: #f74d75;
  animation: dot-3-move 2s ease infinite;
}

@keyframes dot-3-move {
  20% {
    transform: scale(1)
  }

  45% {
    transform: translateY(-18px) scale(.45)
  }

  60% {
    transform: translateY(-90px) scale(.45)
  }

  80% {
    transform: translateY(-90px) scale(.45)
  }

  100% {
    transform: translateY(0px) scale(1)
  }
}
```

一开始它是初始状态，在`45%` 时我们将它缩放为原来的 0.45，再将它向上移动 `18px`，这就是动画开始时一个圆分裂成三个圆的停顿时刻；在 `60%` 向上移动到了最高点 `90px`；最后在回到原位同时缩放会原来的大小。

同理 `.dot-2` 和 `.dot-1` 也经历类似的动画。

```css
.dot-2 {
  background-color: #10beae;
  animation: dot-2-move 2s ease infinite;
}

.dot-1 {
  background-color: #ffe386;
  animation: dot-1-move 2s ease infinite;
}

@keyframes dot-2-move {
  20% {
    transform: scale(1)
  }

  45% {
    transform: translate(-16px, 12px) scale(.45)
  }

  60% {
    transform: translate(-80px, 60px) scale(.45)
  }

  80% {
    transform: translate(-80px, 60px) scale(.45)
  }

  100% {
    transform: translateY(0px) scale(1)
  }
}

@keyframes dot-1-move {
  20% {
    transform: scale(1)
  }

  45% {
    transform: translate(16px, 12px) scale(.45)
  }

  60% {
    transform: translate(80px, 60px) scale(.45)
  }

  80% {
    transform: translate(80px, 60px) scale(.45)
  }

  100% {
    transform: translateY(0px) scale(1)
  }
}
```

此时，我们得到了如下效果。

![1.gif](https://cdn.nlark.com/yuque/0/2022/gif/22793328/1659847296242-b58ec058-5003-4e5f-86bc-750b73f7abe6.gif#clientId=u5249113b-b2de-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=242&id=u861b2257&margin=%5Bobject%20Object%5D&name=1.gif&originHeight=303&originWidth=507&originalType=binary&ratio=1&rotation=0&showTitle=false&size=51367&status=done&style=none&taskId=u44b65d62-c056-41c9-b80c-1b5a9b3e8ef&title=&width=405.6)

接下来便是旋转这三个圆，最方便的做法便是将容器进行旋转，但有一个地方需要注意，我们是要等三个圆移动到最远的位置后开始旋转。

```css
.container {
  width: 200px;
  height: 200px;
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  animation: rotate-move 2s ease-in-out infinite;
}

@keyframes rotate-move {
  55% {
    transform: translate(-50%, -50%) rotate(0deg)
  }

  80% {
    transform: translate(-50%, -50%) rotate(360deg)
  }

  100% {
    transform: translate(-50%, -50%) rotate(360deg)
  }
}
```

**注意：**由于我们一开始使用了 `translate(-50%, -50%)` 来水平垂直居中，所以我动画过程中我们必须得保持这个状态。

如此，我们便得到了以下效果。

![GIF.gif](https://cdn.nlark.com/yuque/0/2022/gif/22793328/1659847787686-07621ee4-c923-40cc-b38f-2c3f589049d9.gif#clientId=u5249113b-b2de-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=242&id=u57a6b294&margin=%5Bobject%20Object%5D&name=GIF.gif&originHeight=303&originWidth=507&originalType=binary&ratio=1&rotation=0&showTitle=false&size=58940&status=done&style=none&taskId=ub7447997-d0fd-47c2-b067-a330424bca2&title=&width=405.6)

到这里便结束了吗？不，细心的读者可能会发现这和开头的动画还是有一些出入。关键在于三个圆分离的时候，我们通过模糊和滤镜来实现：

```html
<svg xmlns="http://www.w3.org/2000/svg" version="1.1">
  <defs>
    <filter id="goo">
      <feGaussianBlur in="SourceGraphic" stdDeviation="10" result="blur" />
      <feColorMatrix in="blur" mode="matrix" values="1 0 0 0 0  0 1 0 0 0  0 0 1 0 0  0 0 0 21 -7" />
    </filter>
  </defs>
</svg>
```

```css
.container {
  width: 200px;
  height: 200px;
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  filter: url('#goo');
  animation: rotate-move 2s ease-in-out infinite;
}
```

如此我们便实现了最终效果：

![](https://cdn.nlark.com/yuque/0/2022/gif/22793328/1659837927616-3027ba48-98fd-45eb-94ea-6fe4505104c5.gif#crop=0&crop=0&crop=1&crop=1&from=url&id=EOpwr&margin=%5Bobject%20Object%5D&originHeight=409&originWidth=607&originalType=binary&ratio=1&rotation=0&showTitle=false&status=done&style=none&title=)


