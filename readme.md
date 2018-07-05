##vue-previewer

###一个基于vue的图片预览插件.

#### 安装 `npm i @redbuck/vue-preview`
#### 使用 `import Previewer from '@redbuck/vue-preview'`
```
<previewer :src="path" :ratio="0.5" :scale="1.8" ratio-ranger="1,10"></previewer>
```

#### props
<table style="text-align: center">
  <thead>
    <tr>
        <td>名称</td>
        <td>功能</td>
        <td>默认值</td>
        <td>类型</td>
        <td>可选值/说明</td>
    </tr>
  </thead>
<tbody>
 <tr>
        <td>src</td>
        <td>图片的地址</td>
        <td>无</td>
        <td>String</td>
        <td>url 地址 || base64 || blob</td>
    </tr>
 <tr>
        <td>size</td>
        <td>容器的大小</td>
        <td>wrap</td>
        <td>String</td>
        <td>实际上是容器的类名,wrap表示由父容器决定</td>
    </tr>
 <tr>
        <td>ratio</td>
        <td>小窗与容器的比例</td>
        <td>0.5</td>
     <td>Number</td>
        <td>默认缩放比例</td>
    </tr>
 <tr>
        <td>scale</td>
        <td>预览窗口与容器的比例</td>
        <td>1</td>
     <td>Number</td>
        <td>number</td>
    </tr>
 <tr>
        <td>ratioRanger</td>
        <td>允许用户缩放的范围</td>
        <td>1,9</td>
     <td>String</td>
        <td>逗号分隔的两个小于10的值</td>
    </tr>
</tbody>
</table>
