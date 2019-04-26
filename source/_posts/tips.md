---
title: 工作中总结的问题
date: 2019-04-26 09:56:55
tags:
---

## 各种杂项

### meta 怎么写

```html
<meta http-equiv="Content-Type" content="text/html; charset=utf-8" />
<meta http-equiv="X-UA-Compatible" content="IE=edge" />
<meta name="renderer" content="webkit" />
<meta name="viewport" content="width=device-width, initial-scale=1" />
<meta name="fragment" content="!" />
<meta name="keywords" content="政府采购网, 政府采购, 招标, 投标" />
<meta name="description" content="政府采购网" />
```

### 禁止 a 标签加上 disabled 后还可以点击

```css
a[disabled] {
  pointer-events: none;
}
```

### 防止在打印时出现链接地址

```less
@media print {
  a[href]:after {
    content: none !important;
  }
}
```

### 文字过长显示省略号(除火狐)

```css
.ellipsis {
  overflow: hidden;
  text-overflow: ellipsis;
  -o-text-overflow: ellipsis;
  white-space: nowrap;
  word-break: keep-all;
}
```

### 防止 ie 自动弹出确认框

```javascript
window.opener = null;
window.open('', '_self', '');
window.close();
```

### 点击下载

```javascript
var downloadFile = function(fileName, filePath) {
  var form = $("<form style='display:none'></form>");
  form.appendTo('body');
  form
    .attr('action', '${devhost}/fd0.action?filePath=' + filePath + '&fileName=' + fileName)
    .attr('method', 'POST');
  form.submit();
};
```

### 遇到资源路径的问题时，使用 base 标签

```html
<base href="/admin-app/" />
```

### delete 是 ie8 的关键字，方法调用时写成这样的形式

```javascript
xxx['delete'].xxx();
```

### 解决 ie8 不支持 background-size:cover; 图片覆盖

```css
filter: progid:DXImageTransform.Microsoft.AlphaImageLoader(src='*.jpg', sizingMethod='scale');
```

### 解决 ie8 不能访问 https 协议网站

1. [失效了。。。](http://playkid.blog.163.com/blog/static/5628726020141236645361/)
2. ssllabs --> 客户端和服务器端加密算法不一致，需要在服务器端进行配置

### ie8 不支持 new Date(yyyy-mm-dd)

`new Date(yyyy-mm-dd.replace(/-/g, '/'))`

### ie8 不支持 Access-Control-Allow-Origin: \* 会导致跨域问题

1. 使用 jsonp
2. ie8 手动设置允许跨域

### window.open 打开 ajax 返回的链接会被拦截，用以下办法处理

```javascript
var newTab = window.open('about:blank');
xxx.get().then(function(data) {
  //使用replace，新页面不会出现后退，如果用 location.href='xxx'，会有后退
  newTab.location.replace(data.content.content);
});
```

### ie iframe 透明

`<iframe allowTransparency="true" />` iframe 里 body 也要设为背景透明

### https 跳转到 http 强制带上 referrer

> referrer 属性可返回载入当前文档的文档的 URL `<meta content="always" name="referrer">`

### 函数注释写法 [jsdoc](http://yuri4ever.github.io/jsdoc/)

```javascript
/**
 * @func 标识一个函数
 * @desc 详细描述和说明
 * @global 全局标识
 * @param {number} a - 参数a
 * @returns {boolean} 返回值
 */
```

### 按键时转换小写转大写

```html
<input onkeyup="this.value = this.value.toUpperCase().trim();" />
```

### 关于跨域

[跨域资源共享 CORS 详解](http://www.ruanyifeng.com/blog/2016/04/cors.html)
[Enable CORS](https://enable-cors.org/index.html)

### 垂直居中

```html
<div class="img_wrap">
  <img src="wgs.jpg" />
</div>
.img_wrap{ // 不能加浮动（若增加一个父级元素） width: 400px; height: 300px; display: table-cell;
//主要是这个属性 vertical-align: middle; text-align: center; }
```

### 离开或刷新页面进行提示

```javascript
//检测离开页面提醒操作
var UnloadConfirm = {};
UnloadConfirm.MSG_UNLOAD = '确定要放弃编辑吗？您离开后数据将不会被保存。';
UnloadConfirm.set = function(text) {
  window.onbeforeunload = function(e) {
    e = e || window.event;
    // 兼容ie8 ie8的退出显示这里的数据
    e.returnValue = '确定要放弃编辑吗？您离开后数据将不会被保存。';
    return text;
  };
};
UnloadConfirm.clear = function() {
  window.onbeforeunload = function() {};
};
UnloadConfirm.set(UnloadConfirm.MSG_UNLOAD);

//选择离开才会执行
window.onunload = function whenUnload() {
  // TODO
};
```

### UMD: 通用模块规范

```javascript
(function(root, factory) {
  if (typeof define === 'function' && define.amd) {
    // AMD
    define(['jquery'], factory);
  } else if (typeof exports === 'object') {
    // Node, CommonJS之类的
    module.exports = factory(require('jquery'));
  } else {
    // 浏览器全局变量(root 即 window)
    root.returnExports = factory(root.jQuery);
  }
})(this, function($) {
  //    方法
  function myFunc() {}

  //    暴露公共方法
  return myFunc;
});
```

### 电话号码中间四位变 \*

```javascript
function hideMobileMid(mobile) {
  var reg = /^(\d{3})\d{4}(\d{4})$/;
  return mobile.replace(reg, '$1****$2');
}
```

### 防止 body 滚动，且 body 带有滚动条

```css
/* 为body添加样式 */
.fixed-body-hook {
  position: fixed;
  width: 100%;
  height: 100%;
  left: 0;
  overflow-y: scroll;
}
/* 或 */
.fixed-body-hook {
  overflow: hidden;
  /* position: relative; */
  width: calc(100% - 17px);
}
```

```javascript
// 打开弹窗时，阻止body滚动
top = $(document).scrollTop(); // 获取当前滚动偏移量
$body.css('top', -1 * top); // 为body设置top
$body.addClass('fixed-body-hook'); // 为body增加fixed样式
// 还原body滚动
$body.removeClass('fixed-body-hook');
$body.css('top', '');
$(document).scrollTop(top);
```

### 最新手机正则

[ChinaMobilePhoneNumberRegex](https://github.com/VincentSit/ChinaMobilePhoneNumberRegex)

### 获取图片原始大小

```javascript
function getNaturalSize(element) {
  var natureSize = {};
  if (element.naturalWidth && element.naturalHeight) {
    natureSize.width = element.naturalWidth;
    natureSize.height = element.naturalHeight;
  } else {
    var img = new Image();
    img.src = element.src;
    natureSize.width = img.width;
    natureSize.height = img.height;
  }
  return natureSize;
}
```

### 获取 iframe 高度

`ele.contentWindow.document.documentElement.scrollHeight`

### 千分位

```javascript
Number('1234564').toLocaleString();
vartoThousands = function(number) {
  return (number + '').replace(/(\d)(?=(\d{3})+$)/g, '$1,');
};
```

### 防止默认填充密码

```html
<!-- 隐藏的input, 为了禁用浏览器的用户名密码表单自动填充 -->
<input type="password" style="height: 0;padding: 0;width: 0;border: 0;position: absolute;" />

<!-- password上加属性 -->
<input type="password" autocomplete="new-password" />
```

### 生成证书

```
第一步:
打开: https://myssl.com/create_test_cert.html

第二步:
填写参数:
证书类型: 服务器证书
加密算法: RSA
域名: gmsoft.com (这个不重要, chrome58 以后不会使用它来验证域名了)

高级设置:
备用名称: 填写要注册的域名(最后列出了目前注册的, 可以在此基础上增加 更新证书)

密钥强度: 2048
签名哈希算法: SHA256

--------------------------------------------------------------------------------

目前证书注册的域名, 如有需要请增加并重新生成证书:
*.gmsoft.com,*.cqzcjdev.com,*.cqzcjshow.com,*.cqzcjtest.com,*.cqzcjtestoper.com,*.gpwdevcom,*.gpwtest.com,*.gpwshow.com,*.gpwtestoper.com,*.culturedev.com,*.culturetest.com,*.culturetestoper.com,*.cultureshow.com,*.wlwculture.com,*.csmhtest.com,*.lgmhtest.com,*.yzmhtest.com,*.dgmhtest.com.conf
```

### 根据 id 从树结构中取对应节点，利用 throw 跳出递归

```javascript
const getNodeById = (id, menus) => {
  // 定义变量保存当前结果路径
  let result;
  function getNode(nodes) {
    forEach(nodes, item => {
      // 找到符合条件的节点，通过throw终止掉递归
      if (item.key === id) {
        result = item;
        throw 'GOT IT!';
      }
      if (item.children && item.children.length > 0) {
        getNode(item.children);
      }
    });
  }
  try {
    getNode(menus);
  } catch (e) {
    console.log(e);
  }
  return result;
};
```

## angular1.x

### 用\$last 控制最后一个元素的 class

```html
<li ng-repeat="rule in rules | orderBy:\'-date\'" class="ellipsis"' 'ng-class="{\'last\':$last}"
ui-sref="rules.detail({id: rule.id})">
```

### 绑定 html

```javascript
app.filter('to_trusted', ['$sce', function ($sce) {
  return function (text) {
      return $sce.trustAsHtml(text);
  };
}]);
html code:
<p ng-bind-html="currentWork.description | to_trusted"></p>
```

### angular 的 JSON 字符串转对象

```javascript
s.str = angular.fromJson(s.jsonStr);
s.json = angular.toJson(s.obj);
```

### 用 angular 省略号：

```html
<label ng-if="!item.expand" style="word-break: break-all;font-weight: normal">
  {{item.content | limitTo:120}}<span ng-if="item.content.length>120">....</span>
  <a ng- if="item.content.length>120" ng-click="item.expand=true">更多</a></label
>
<label ng-if="item.expand" style="word-break: break-all;font-weight: normal"
  >{{item.content }}<a ng-click="item.expand=false">收起</a></label
>
```

### ng-option 的使用，可以绑定 ng-model 的值为 as 前面的属性的值

```html
<select
  class="form-control"
  ng-model="marketId"
  ng-options="a.id as a.adminAreaName for a in areas"
>
  <option value="">--请选择--</option>
</select>
```

> 这里有一个坑，如果要初始化选中项，ng-model 必须是 String 类型

### 为异步加载的元素绑定事件,放在 \$timeout 里

```javascript
$timeout(function() {
  var width;
  $('.ellipsis').each(function() {
    width = $(this)
      .css('width')
      .slice(0, -2);
    if ($(this)[0].scrollWidth > width) $(this).tipsy({ fade: true });
    else {
      $(this).removeAttr('title');
    }
    $(this).click(function() {
      $('.tipsy').remove();
    });
  });
});
```

### ng-if 的坑

> ng-show 和 ng-hide 是不自带作用域的，而 ng-if 则自己创建了一级作用域。在用的时候，两者就是有差别的，比如说内部元素访问外层定义的变量，就需要使用\$parent 语法了。

### angular 自定义验证

```javascript
var 验证方法 = function(输入){
        ... //对输入验证
        s.editform.provider.$setValidity('has', false);
    }
    s.editform.provider.$parsers.push(验证方法);
    s.editform.provider.$formatters.push(验证方法);
```

### angular 阻止事件冒泡

```javascript
<div ng-click="doSomething($event)" />;
s.doSomething = function(e) {
  e.stopPropagation();
};
```

### angular 自定义指令函数传参

[angular 自定义指令作用域&--传递引用](http://blog.csdn.net/inuyasha1121/article/details/71249156)

### angular 自定义指令 scope 里不能用 name，和 form 验证有冲突

### angular 的几种 watch

1. $watchGroup 的第一个参数要传数组，$watchCollection 的第一个参数要传对象
2. 监视对象和数组而且监视层次不是特别深，优先使用$watchCollection, $watchCollection 可以方便的监视数组的插入，移除。
3. 要同时监视多个变量并执行同一逻辑使用\$watchGroup
4. 除此一般情况下使用\$watch,如果你知道数据结构的深度，可以直接这样监视。 当第三个参数 true，表示深度监测，如果 watch 的变量比较复杂，效率会变低。

## 好用的 npm 包

- nodemon 自动重新启动服务器
- rimraf 删除文件
- cross-env 跨平台地设置及使用环境变量
- concurrently 同时启用多个持久服务
- fkill 跨平台杀掉进程
- install-peerdeps 同时安装包的依赖
- husky 在 commit 之前执行其他操作，如 lint
- react-helmet 为 react 项目设置头部
- npx 从本地 node_modules/.bin 中执行命令
- emma-cli 搜索方式安装包

## 持续更新中...
