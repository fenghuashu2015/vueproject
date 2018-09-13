# v-sport

> A Vue.js project

## Build Setup

``` bash
# install dependencies
npm install

# serve with hot reload at localhost:8080
npm run dev

# build for production with minification
npm run build

# build for production and view the bundle analyzer report
npm run build --report
```

第一次使用vue构建项目遇到很多问题，以下只是一小部分，选择信息展示类网站难度系数不高适合练手.

>项目中遇到的问题解决   

1. vue-lazyload插件使用

   + 动态切换图片时dom绑定图片不变问题,需要给图片绑定key值   
  `<img v-lazy="localdata+'?w=240'" class="localPic" :key="localdata" />`
   + img标签设置默认加载图片可在样式中`img[lazy="loading"]`设置自定义loading图   
        ```
        img[lazy="loading"] {
        width:240px;
        height: 360px;
        background-position: center 0 !important;
        background: url("../assets/images/defaultImg.png");
        background-repeat: no-repeat;
        -webkit-background-size: cover;
        -moz-background-size: cover;
        -o-background-size: cover;
        background-size: cover;
        }
        img[lazy=error] {}
        img[lazy=loaded] {}
        ```
   + 背景图形式`<div v-lazy:background-image="imgUrl"></div>`设置默认加载图片
        ```
        .itemimg[lazy=loading]{
            background:#f2f2f2;
            background-repeat: no-repeat;
            background-size: cover;
        }
        .itemimg[lazy=error] {}
        .itemimg[lazy=loaded] {}
        ```
2. elementUI使用问题   
   + 组件点击事件无效，需添加.native修饰符，@click.native
   + el-table-column中嵌套组件需包一层`<template scope="scope"></template>`
   + table组件中，
        ```
        <template scope="scope">
            <el-button size="small" @click="click(scope.$index, scope.row)">点击文字</el-button>
        </template>
            
        <el-table-column  label="预览" prop="dataurl" header-align="center">
            <template scope="scope">
                <img :src="scope.row.dataurl"/>
            </template>
        </el-table-column>
        ```
3. vue-cli构建项目
   + 使用filters局部自定义过滤器时，需考虑val为null的 情况，否则报错
        ```
        if (val != null) {
            let reg = /(\d{4})(?=\d{2})/g;
            let str = val.toString();
            str = str.replace(reg, "$1-");
            return str;
        }
        ```
    + 动态使用静态图片需将静态图片放在`static/img`目录下，在组件中使用路径均为`../static/img`或将静态图片转换为base64位图片
    + 处理新闻返回编辑器html标签字符串，使用`v-html`解析，本例中需要对返回dom节点进行操作，采用原生js方法`document.getElementById(name).innerHTML=内容`进行解析
    + 对`v-html`解析成的标签添加样式，写样式时需添加>>>，例`.foo >>> img { max-width: 100%; }`
4. axios发送post请求时，遇到三种情况
    + 无data参数：
        ```
        axios.post(`resturl/${参数1}/${参数2}`,{},{
            headers: {
                'appId': 99
            }
        })
        ```
    + data参数序列比：
        ```
        axios.post(`resturl/${type}`,JSON.stringify({pageSize:5,pageNo:1,needPage:true}),{
            headers: {
                'Content-Type': 'application/json;charset=UTF-8'
            }
        })
        ```
    + 使用axios自带模块qs：
        ```
        axios.post(`resturl`,qs.stringify({
            "NhId":nhid,
            "MobileNumber":mobile,
            "PlayerId":votearr
        }),{
            headers:{
                'Content-Type': 'application/x-www-form-urlencoded;charset=UTF-8'  
            }
        })
        ```




