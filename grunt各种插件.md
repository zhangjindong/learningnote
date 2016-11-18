[TOC]

grunt的各种插件
==========

## [grunt-wiredep](https://www.npmjs.com/package/grunt-wiredep)

####What is this?

> Automatically inject Bower components into the HTML file
> 自动把Bower的组件注入到HTML文件中


####实例
* 分别安装组件
* 
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

----------------------------------------------------------------
## [grunt-contrib-watch](https://www.npmjs.com/package/grunt-contrib-watch)

####What is this?

>	它是一个监听Task并执行对应的Task，watch在Gruntfile.js中有一个配置参数.
>	比如如果我们配置了一个copy的Task，告知watch如果有copy指定的文件修改了，如果监听到那些文件，就立即执行copy Task.

####根据官网安装步骤

*	首先：安装插件：

		npm install grunt-contrib-watch --save-dev

*	在Gruntfile.js文件中加载Npm插件任务

		grunt.loadNpmTasks('grunt-contrib-watch');  
	
* 在Gruntfile.js中配置watch，我们在之前学习安装Grunt时用到的grunt-contrib-copy插件的基础上来操作。

		watch: {  
		copy: {  
		   files: '<%=config.app%>/**/*.html',  
		   tasks: ['copy:dest']  
		} 
