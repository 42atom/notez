# 如何用 framer 做响应式布局:  

 https://uxtools.co/blog/how-to-use-framer-for-responsive-web-design



# 图层对象的选择API

```coffeescript
# Get an array of all layers in your project
Layer.selectAll('*')

# Get a specfic layer based on layer name
Layer.select('image')

# All layers named 'image' and direct descendants of layers named 'card'
Layer.selectAll('card' > 'image')

# All layers with names starting with 'card'. eg. card1,card2,card3 ...
Layer.selectAll('card*')

# Get a specfic child from myLayer
myLayer.selectChild('button')

# Get all children named image
myLayer.selectChildren('image')


#### 例- 在使用flow的时候, 添加返回按钮时, 以前:
for btn in [back_btn1, back_btn2, back_btn3, back_btn4, back_btn5]
  btn.onClick -> flow.showPrevious()

# 作为替代,可以这样:
for btn in layer.selectAll('back_btn*')
  btn.onClick -> flow.showPrevious()
```

---



