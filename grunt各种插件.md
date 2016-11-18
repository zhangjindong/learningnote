[TOC]

grunt的各种插件
==========

##1. [grunt-wiredep](https://www.npmjs.com/package/grunt-wiredep)
----------------------------------------------------------------

####What is this?
> Automatically inject Bower components into the HTML file
> 自动把Bower的组件注入到HTML文件中

####实例
* 分别安装组件
> bower install jquery --save 
> bower install sea.js --save
* 安装后会在bower.json文件中看到

		{  
			"name": "framework-demo",  
			"private": true,  
			"dependencies": {  
			"jquery": "~2.1.4",  
			"jquery-ui": "~1.11.4",  
			"knockout": "^3.3.0",  
			"seajs": "^3.0.0"  
			}

* 在Gruntfile.js文件中配置wirdep Task

		wiredep: {  
		    app: {  
		      src: ['<%= config.app %>/index.html'],  
		      ignorePath: /^(\.\.\/)*\.\./  
		    }  
		  }

* 执行grunt wiredep命令
* 在index.html文件会把默认dependencies依赖中的组件自动注入到下面标签中去。

        <!-- bower:js -->  
           <script src="/bower_components/jquery/dist/jquery.js"></script>  
           <script src="/bower_components/jquery-ui/jquery-ui.js"></script>  
           <script src="/bower_components/knockout/dist/knockout.js"></script>  
           <script src="/bower_components/seajs/dist/sea.js"></script>   
        <!-- endbower -->
