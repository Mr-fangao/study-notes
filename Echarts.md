### Echarts样式修改

#### 一、折线图

###### 1.1坐标轴设置

```javascript
axisLine: {
            lineStyle: {
              // 设置x、y轴颜色
              color: '#989DA1'
			}
}
```

###### 1.2 布局设置

```javascript
 grid: {
          top: "0%", //距上边距
          left: "0%", //距离左边距
          right: "0%", //距离右边距
          bottom: "0%", //距离下边距
        },
 legend: {
          // orient 设置布局方式，默认水平布局，可选值：'horizontal'（水平） ¦ 'vertical'（垂直）
          orient: 'horizontal',
          // x 设置水平安放位置，默认全图居中，可选值：'center' ¦ 'left' ¦ 'right' ¦ {number}（x坐标，单位px）
          x: 'left',
          // y 设置垂直安放位置，默认全图顶端，可选值：'top' ¦ 'bottom' ¦ 'center' ¦ {number}（y坐标，单位px）
          y: 'top',
          data: ['预期','实际','假设']
        },
```

###### 1.3折现样式设置

```javascript
itemStyle : {  
                                normal : {  
                                    //折线点颜色
                                    color:'#00FF00',  
                                    lineStyle:{  
                                        //折线颜色
                                        color:'#00FF00'  
                                    }  
                                }  
                            },  
```

###### 1.4title

```javascript
 title : {
            show:true, //显示策略，默认值true,可选为：true（显示） | false（隐藏）
            text: '主标题', //主标题文本，'\n'指定换行
            link:'', //主标题文本超链接,默认值true
            target: null, //指定窗口打开主标题超链接，支持'self' | 'blank'，不指定等同为'blank'（新窗口）
            subtext: '副标题', //副标题文本，'\n'指定换行
            sublink: '', //副标题文本超链接
            subtarget: null, //指定窗口打开副标题超链接，支持'self' | 'blank'，不指定等同为'blank'（新窗口）
            x:'center' //水平位置，默认为'left'，可选为：'center' | 'left' | 'right' | {number}（x坐标，单位px）
            y: 'top', //垂直安放位置，默认为top，可选为：'top' | 'bottom' | 'center' | {number}（y坐标，单位px）
            textAlign: null ,//水平对齐方式，默认根据x设置自动调整，可选为： left' | 'right' | 'center
            backgroundColor: 'rgba(0,0,0,0)', //标题背景颜色，默认'rgba(0,0,0,0)'透明
            borderColor: '#ccc', //标题边框颜色,默认'#ccc'
            borderWidth: 0, //标题边框线宽，单位px，默认为0（无边框）
            padding: 5, //标题内边距，单位px，默认各方向内边距为5，接受数组分别设定上右下左边距
            itemGap: 10, //主副标题纵向间隔，单位px，默认为10
            textStyle: { //主标题文本样式{"fontSize": 18,"fontWeight": "bolder","color": "#333"}
                fontFamily: 'Arial, Verdana, sans...',
                fontSize: 12,
                fontStyle: 'normal',
                fontWeight: 'normal',
            },
            subtextStyle: {//副标题文本样式{"color": "#aaa"}
                fontFamily: 'Arial, Verdana, sans...',
                fontSize: 12,
                fontStyle: 'normal',
                fontWeight: 'normal',
            },
            zlevel: 0, //一级层叠控制
            z: 6, //二级层叠控制，默认6,同一个canvas（相同zlevel）上z越高约靠顶层。
        },

```
###### 1.5图例
```javascript
legend: {
        data:['临停车','月租车','免费车','储值车','军警车'],
        left:'center',
        bottom:'10%',
        itemWidth:10,//图例的宽度
        itemHeight:10,//图例的高度
        textStyle:{//图例文字的样式
            color:'#ccc',
            fontSize:16
        }
    },
```
