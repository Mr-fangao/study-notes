### Code

##### 一、el-tab加载echarts问题

```html
<el-tabs
        v-model="activeName2"
        type="border-card"
        @tab-click="handleTabClick"
      >
</el-tab>
```

```javascript
handleTabClick(tab) {
      console.log(tab.name);
      if (tab.name == "time") {
        setTimeout(() => {
          this.initTimechart();
        }, 500);
      }
},
```

##### 二、