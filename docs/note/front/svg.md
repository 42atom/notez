# SVG 多行文字自动换行的处理：

```
<svg width="200" height="200" overflow="visible">
  <foreignObject x="0" y="0" overflow="visible" width="200" height="200"
  requiredFeatures="http://www.w3.org/TR/SVG11/feature#Extensibility">
    <div style="width: 200px; height: 200px; padding:0px; opacity:0.5;font-family:Verdana;
    font-size:14px; white-space: normal; word-wrap: normal; overflow: visible;"
    xmlns="http://www.w3.org/1999/xhtml">

    与所有非亚洲语言的normal相同。对于中文，韩文，日文，不允许字断开。适合包含少量亚洲文本的非亚洲文本适合包含少量亚洲文本的非亚洲文本适合包含少量亚洲文本的非亚洲文本适合包含少量亚洲文本的非亚洲文本适合包含少量亚洲文本的非亚洲文本
    与所有非亚洲语言的normal相同。对于中文，韩文，日文，不允许字断开。适合包含少量亚洲文本的非亚洲文本适合包含少量亚洲文本的非亚洲文本适合包含少量亚洲文本的非亚洲文本适合包含少量亚洲文本的非亚洲文本适合包含少量亚洲文本的非亚洲文本

    </div>
  </foreignObject>
</svg>


```



