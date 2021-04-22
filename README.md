# Lascyb 模板引擎

## 主要特性
- 基于 ThinkTemplate 二次开发
- 加入模板常量配置文件；
- 模板常量文件文件后缀支持（.json）；
- 支持主题模板
- ```php
  //在view 增加参数
  "view_config_suffix"=>"json" // 默认模板配置后缀，其他后缀解析暂未完成
  ```
- 使用方法
    >如定义了配置文件，则根据对应的文件名进行调用，
    > 例如有 index.html 文件 ，存在配置文件index.json
    ```json
    {
      "name":"我的文件全名叫做 index.html"    
    }
    ```
    >调用时在index.html 使用 __INDEX.name 即可调用
    > >会在模板缓存中作为常量直接缓存
  
- 其他特性参考下面 ThinkTemplate

## 安装(一般需配合 lascyb/think-view 使用)
~~~php
composer require lascyb/think-template
~~~

>---
>相关用法基本与 ThinkTemplate 相同,但是在视图文件夹中增加了配置文件，和主题模板目录，目录结构如下
>
 - comtroller
 - model
 - view 
    - default
        - index
            - index.html
            - index.json
        - public
            - header.html
            - header.json
            - footer.html
            - footer.json
    - red
        - index
            - index.html
            - index.json
> - 以上定义了一个default 主题 和 red 主题，并分别为模板文件定义了配置文件
> - 需要注意的是，如果index.html 引用了 header.html 或者使用了模板布局，
    如果各个模板文件的名称不相同则无妨，但若是存在文件名相同的模板，则会存在优先级问题(
    当前模板文件 > include标签包含的模板文件 > extend标签包含的模板文件 
    > 布局标签包含的模板文件 > 模板布局文件 > 视图文件配置的tpl_replace_string)  
> - 若启用主题模板，需要在试图配置(一般是view.php)中添加theme 配置 如

```php 
return [
    // 模板引擎类型使用Lascyb
    'type'          => 'Lascyb',
    // 主题模板使用 default，不使用填 false
    'theme'         =>'default'
];
```
>
>
>
>>>---
# ThinkTemplate

基于XML和标签库的编译型模板引擎

## 主要特性

- 支持XML标签库和普通标签的混合定义；
- 支持直接使用PHP代码书写；
- 支持文件包含；
- 支持多级标签嵌套；
- 支持布局模板功能；
- 一次编译多次运行，编译和运行效率非常高；
- 模板文件和布局模板更新，自动更新模板缓存；
- 系统变量无需赋值直接输出；
- 支持多维数组的快速输出；
- 支持模板变量的默认值；
- 支持页面代码去除Html空白；
- 支持变量组合调节器和格式化功能；
- 允许定义模板禁用函数和禁用PHP语法；
- 通过标签库方式扩展；

## 安装

~~~
composer require topthink/think-template
~~~

## 用法示例


~~~php
<?php
namespace think;

require __DIR__.'/vendor/autoload.php';

// 设置模板引擎参数
$config = [
	'view_path'	=>	'./template/',
	'cache_path'	=>	'./runtime/',
	'view_suffix'   =>	'html',
];

$template = new Template($config);
// 模板变量赋值
$template->assign(['name' => 'think']);
// 读取模板文件渲染输出
$template->fetch('index');
// 完整模板文件渲染
$template->fetch('./template/test.php');
// 渲染内容输出
$template->display($content);
~~~

支持静态调用

~~~
use think\facade\Template;

Template::config([
	'view_path'	=>	'./template/',
	'cache_path'	=>	'./runtime/',
	'view_suffix'   =>	'html',
]);
Template::assign(['name' => 'think']);
Template::fetch('index',['name' => 'think']);
Template::display($content,['name' => 'think']);
~~~

详细用法参考[开发手册](https://www.kancloud.cn/manual/think-template/content)