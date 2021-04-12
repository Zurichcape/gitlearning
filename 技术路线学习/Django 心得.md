## Django 开发心得
---
### Django的优点
- 完备性：遵循"功能完备"理念，遵循一致性设计原则。
- 通用性：构建任何类型的网站，并与任何客户端框架一起工作，提供任何格式(HTML,RSS源，JSON,XML)的内容。
- 安全性：设计了自动保护网站的框架，可以避免如XSS（cross site scripting）攻击。
  1. django使用了**escape过滤器**来避免HTML的一些标记可能会影响渲染结果(该过滤器是默认开启的，如果有些元素需要被HTML渲染到网页上，可以手动设置)。
  
	__for individual variables:__   

		`This will be escaped: {{ data }}`  
		`This will not be escaped: {{ data|safe }}`
		`This will be escaped: &lt;b&gt;`  
		`This will not be escaped: <b>`  
	 
	__for template blocks__:  
	
		`{% autoescape off %}`  
    	`Hello {{ name }}`  
		`{% endautoescape %}`
	
		Auto-escaping is on by default. Hello {{ name }}
		{% autoescape off %}
    		This will not be auto-escaped: {{ data }}.
    		Nor this: {{ other_data }}
    		{% autoescape on %}
    		    Auto-escaping applies again: {{ name }}
    		{% endautoescape %}
		{% endautoescape %}  
  
	__auto-escape 选项(on/off)会通过·extend·传递给子类或者通过`include`标签传递__  

		base.html
		{% autoescape off %}
		<h1>{% block title %}{% endblock %}</h1>
		{% block content %}
		{% endblock %}
		{% endautoescape %}
		
		child.html
		{% extends "base.html" %}
		{% block title %}This &amp; that{% endblock %}
		{% block content %}{{ greeting }}{% endblock %}
		
		{% include "foo/base.html" %}#也会传递escape
	__string literals__  
		
		所有字符串变量都会不经过自动变换(auto-escape)直接写入网页中，因此你可以这样写：  
	    {{ data|default:"3 &lt; 2" }}
		而不是：
		{{ data|default:"3 < 2" }} #这是错误的写法因为符号<会被直接写入  

	__django的保护也是有限的，比如以下这种情况就无法保护。__ 
 
		<style class={{ var }}>...</style>
		var = 'class1 onmouseover=javascript:func()' #会导致未经授权的js执行
	CSRF


- 可扩展性：基于组件的无共享架构（架构的每一个部分独立于其他架构，可以根据需要进行替换和更改).在不同部分之间有明确的分隔意味着可以在任何级别添加硬件来扩展服务：缓存数据库，数据库服务器或应用程序服务器。
- 可维护性：遵循设计原则和模式，鼓励创建可维护和可重复使用的代码。使用了不要重复自己(DRY)原则，将相关功能分组到可重用的"应用程序"中，并且在较低级别将相关代码分组或模块。
- 灵活性：可以在多平台上运行。


![django](https://media.prod.mdn.mozit.cloud/attachments/2016/09/23/13931/9db08f02b23e353bcd1597947e612079/basic-django.png)

