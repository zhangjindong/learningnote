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
####以下是 在配置Grunt的Task时通配符支持和动态生成文件名详解

		copy: {  
		    // 这是Task里的其中一个Target  
		    dests: {  
		      expand: true,  
		      cwd: '<%=config.app%>/newFolder',  
		      src: ['**/{a*,b*}.html'],  
		      dest: '<%=config.dist%>/newFolder',  
		      ext: ".shtml",  
		      extDot: "first",  
		      flatten:true, //去掉中间上当，下面的rename可以再找回来  
		      rename: function( dest, fileName ) {  
		        return dest + "/" +fileName;  
		      }  
		    }  
		  } 
####
		1、*匹配任何字符，除了/
		2、?匹配单个字符，除了/
		3、**匹配任何字符，包括/，所以用在目录路径里面
		4、{}逗号分割的“或”操作(逗号后面不要有空格)
		5、! 排除某个匹配


		动态生成文件名:
		expand 设置为true打开以下选项,如果设为true，就表示下面文件名的占位符（即*号）都要扩展成具体的文件名。

		cwd 所有src指定的文件相对于这个属性指定的路径,需要处理的文件（input）所在的目录

		src 要匹配的路径，相对与cwd,表示需要处理的文件。如果采用数组形式，数组的每一项就是一个文件名，可以使用通配符

		dest 生成的目标路径前缀,表示处理后的文件名或所在目

		ext 表示处理后的文件后缀名。替换所有生成的目标文件后缀为这个属性

		extDot:first：表示以文件名后的第一个点后面开始作为后缀名;last：表示以文件名后的最后一个点后面开始作为后缀名

		flatten:删除所有生成的dest的路径部分,值为boolean类型（true、false）用来指定是否保持文件目录结构,true是保持文件目录

		rename 一个函数，接受匹配到的文件名，和匹配的目标位置，返回一个新的目标路径


----------------------------------------------------------------
## [grunt-contrib-uglify](https://www.npmjs.com/package/grunt-contrib-uglify)

####What is this?

> ① 在src中找到zepto进行压缩（具体名字在package中找到）
> ② 找到dest目录，没有就新建，然后将压缩文件搞进去
> ③ 在上面加几个描述语言

若是你希望给你文件的头部加一段注释性语言配置banner信息即可

    grunt.initConfig({
      pkg: grunt.file.readJSON('package.json'),
      uglify: {
        options: {
          banner: '/*! 注释信息 */'
        },
        my_target: {
          files: {
            'dest/output.min.js': ['src/input.js']
          }
        }
      }
    });

----------------------------------------------------------------
## [grunt-contrib-concat](https://www.npmjs.com/package/grunt-contrib-concat)

####What is this?

> 该插件主要用于代码合并，将多个文件合并为一个，我们前面的uglify也提供了一定合并的功能
> 在可选属性中我们可以设置以下属性：
> ① separator 用于分割各个文件的文字，
> ② banner 前面说到的文件头注释信息，只会出现一次
> ③ footer 文件尾信息，只会出现一次
> ④ stripBanners去掉源代码注释信息（只会清楚/**/这种注释）

####一个简单的例子

    module.exports = function (grunt) {
      grunt.initConfig({
      concat: {
        options: {
          separator: '/*分割*/',
          banner: '/*测试*/',
          footer: '/*footer*/'
         
        },
        dist: {
          src: ['src/zepto.js', 'src/underscore.js', 'src/backbone.js'],
          dest: 'dist/built.js',
        }
      }
    });
      grunt.loadNpmTasks('grunt-contrib-concat');
    }
 合并三个文件为一个，这种在我们源码调试时候很有意义

####构建两个文件夹

有时候我们可能需要将合并的代码放到不同的文件，这个时候可以这样干

    module.exports = function (grunt) {
      grunt.initConfig({
        concat: {
          basic: {
            src: ['src/zepto.js'],
            dest: 'dest/basic.js'
          },
          extras: {
            src: ['src/underscore.js', 'src/backbone.js'],
            dest: 'dest/with_extras.js'
          }
        }
      });
      grunt.loadNpmTasks('grunt-contrib-concat');
    }
这种功能还有这样的写法：

    module.exports = function (grunt) {
      grunt.initConfig({
        concat: {
          basic_and_extras: {
            files: {
              'dist/basic.js': ['src/test.js', 'src/zepto.js'],
              'dist/with_extras.js': ['src/underscore.js', 'src/backbone.js']
            }
          }
        }
      });
      grunt.loadNpmTasks('grunt-contrib-concat');
    }

----------------------------------------------------------------
## [grunt-contrib-jshint](https://www.npmjs.com/package/grunt-contrib-jshint)

####What is this?
> 该插件用于检测文件中的js语法问题，比如我test.js是这样写的：
> alert('我是叶小钗')
> 说我缺少一个分号，好像确实缺少.....如果在里面写明显的BUG的话会报错多数时候，我们认为没有分号无伤大雅，所以，我们文件会忽略这个错误：

    module.exports = function (grunt) {
      grunt.initConfig({
        jshint: { 
            options: {
                '-W033': true   //忽略这个错误
            },
          all: ['src/test.js']
        }
      });
      grunt.loadNpmTasks('grunt-contrib-jshint');
    }

----------------------------------------------------------------
## [grunt-contrib-cssmin](https://www.npmjs.com/package/grunt-contrib-cssmin)

####What is this?
> 样式文件的打包方式与js不太一样，这里我们下载css-min插件，并且在package.json中新增依赖项

    module.exports = function (grunt) {
      grunt.initConfig({
        cssmin: {
          compress: {
            files: {
              'dest/car.min.css': [
              "src/car.css",
              "src/car01.css"
            ]
            }
          }
        }
      });
      grunt.loadNpmTasks('grunt-contrib-cssmin');
    }

----------------------------------------------------------------
## [grunt-contrib-requirejs](https://www.npmjs.com/package/grunt-contrib-requirejs)

####What is this?
> 用户requirejs的合并工作

----------------------------------------------------------------
## [grunt-contrib-connect](https://www.npmjs.com/package/grunt-contrib-connect)

####What is this?
> 用来充当一个静态文件服务器，本身集成了 livereload 功能 ，grunt-contrib-watch , 监视文件的改变，然后执行指定任务，这里用来刷新  grunt serve 打开的页面
> load-grunt-tasks , 省事的插件，有了这个可以不用写一堆的 grunt.loadNpmTasks('xxx') ，再多的任务只需要写一个 require('load-grunt-tasks')(grunt) 。
> 参考的文档中提到了 time-grunt 插件，可用来显示每一个任务所花的时间和百分比，由于此示例中基本就 watch 任务占了百分百的时间。
> require('time-grunt')(grunt); 如果要使用 time-grunt 插件

####实例

  module.exports = function(grunt) {

    require('load-grunt-tasks')(grunt); //加载所有的任务
    //require('time-grunt')(grunt); 如果要使用 time-grunt 插件

    grunt.initConfig({
      connect: {
        options: {
          port: 9000,
          hostname: '*', //默认就是这个值，可配置为本机某个 IP，localhost 或域名
          livereload: 35729 //声明给 watch 监听的端口
        },

        server: {
          options: {
            open: true, //自动打开网页 http://
            base: [
              'app' //主目录
            ]
          }
        }
      },

      watch: {
        livereload: {
          options: {
            livereload: '<%=connect.options.livereload%>' //监听前面声明的端口  35729
          },

          files: [ //下面文件的改变就会实时刷新网页
            'app/*.html',
            'app/style/{,*/}*.css',
            'app/scripts/{,*/}*.js',
            'app/images/{,*/}*.{png,jpg}'
          ]
        }
      }
    });

    grunt.registerTask('serve', [
      'connect:server',
      'watch'
    ]);
  }
现在我们配置好了一个静态文件 Web 服务器，运行命令

grunt serve

会自动打开浏览器访问 http://0.0.0.0:9000, 然后有个 watch 一直在监听文件的改变。如果 app 目录下有 index.html 则浏览该文件，没有索引文件就显示文件目录列表。

本文原始链接 http://unmi.cc/grunt-contrib-connect-build-livereload-dev-env/ , 

这时候在 app 目录中增，删相关类型的文件都会实时反映在上面的 http://0.0.0.0:9000 页面上，也就是文件列表可以实时刷新。

现在我们想要更极致的实时页面内容的预览，我们打开上面的某个页面，如 http://0.0.0.0:9000/test.html，然而我们来编辑 test.html 文件内容，来观察页面是不是能实时预览。

你也应该不能在页面上看到实时的修改，为能实时预览我们还要做点事情，看这里 https://github.com/gruntjs/grunt-contrib-watch#live-reloading 有两种办法：

1. 在需要实时预览的页面里加上

<script src="http://localhost:35729/livereload.js"></script>
注意相应的主机和端口号

2. 安装浏览器扩展，包括 Safari, Chrome 和 Firefox 的，点击链接   How do I install and use the browser extensions? 安装。这样就不用在页面上引入上面的脚本。

现在看实时的预览效果

grunt-connect 还可以和 grunt-connect-proxy 结合来制作本地代理访问其他域名的 api 而不用处理跨域问题，有空再体验下 grunt-connect-proxy。