# select-modal 模态选择组件

Author：@dunizb

## screenshot
![screenshot.png](screenshot.png)

## Demo
[demo.vue](demo.vue)

## Props
|属性名|       说明      |  类型 |是否必须|是否双向绑定|默认值|
|:------|:--------------|:------|:------|:-------|:-----|
|data  |select中的所有选项|Array  |是     |否       | -   |
|title |组件标题头        |String |否     |否       | -   |
|multip|是否支持多选      |Boolean|否     |否       | false|

## Events
|函数名 |       说明      |  参数          |
|:------|:--------------|:--------------|
|change |选择选项之后触发的事件|Object或Array，当props.multip = true时返回Array，否则返回Object  |

## Methods
|方法名 |       说明    |
|:------|:--------------|
|open |显示组件|
|hide |隐藏组件|

## Slot
|Slot |       说明    |
|:------|:--------------|
|top |显示在选项头部|
|bottom |显示在选项的尾部|