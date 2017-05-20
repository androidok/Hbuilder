# Hbuilder
webApp的开发
第一次做项目出现的错误
<head>
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width,initial-scale=1,minimum-scale=1,maximum-scale=1,user-scalable=no" />
    <title></title>
    <script src="js/mui.min.js"></script>
    <link href="css/mui.min.css" rel="stylesheet"/>
       	<link rel="stylesheet" type="text/css" href="css/own.css"/>
   	<link rel="stylesheet" type="text/css" href="css/iconfont.css"/>
   
    <!--设置点击之后的底部导航图标的颜色-->
    	<style type="text/css">
	  	.mui-active .mui-icon,
	  	.mui-active .mui-tab-label {
		    	color: #41bea9;
	  	}
   	</style>
</head>
<body  class="own-gray-color">
	
	<header class="mui-bar mui-bar-nav own-main-background-color">
	    <h1 id="nav-title"  class="mui-title">标题</h1>
	</header>
	<nav class="mui-bar mui-bar-tab">
	    <a class="mui-tab-item mui-active" href="barItem/home.html">
	        <span class="mui-icon iconfont icon-home"></span>
	        <span class="mui-tab-label">首页</span>
	    </a>
	    <a class="mui-tab-item" href="categoryfenlei.html">
	        <span class="mui-icon iconfont icon-fenlei"></span>
	        <span class="mui-tab-label">分类</span>
	    </a>
	    <a class="mui-tab-item" href="barItem/wishList.html">
	        <span class="mui-icon iconfont icon-xinyuandan"></span>
	        <span class="mui-tab-label">心愿单</span>
	    </a>
	    <a class="mui-tab-item" href="barItem/cart.html">
	        <span class="mui-icon iconfont icon-cart"></span>
	        <span class="mui-tab-label">购物车</span>
	    </a>
	    <a class="mui-tab-item" href="barItem/mine.html">
	        <span class="mui-icon iconfont icon-wode"></span>
	        <span class="mui-tab-label">我的</span>
	    </a>
	</nav>
	<script src="js/mui.min.js" charset="UTF-8"></script>
	 <script type="text/javascript" charset="utf-8">
      	mui.init({
      			//启动右滑关闭功能
      			swipeBack:false
      	});
      	var navTitle;
		var mainWebview;
		//记录当前显示的webview
		var curBarItemWebview;
		//存放预加载完成的webview
		var barItemWebviewArray = [];
		//出现错误的主要原因是存储的html和显示的路径不一样
		var barItemArray = ['barItem/home.html','categoryfenlei.html','barItem/wishList.html','barItem/cart.html','barItem/mine.html'];
		mui.plusReady(function(){
			//获取当前的title
			navTitle=document.querySelector(".mui-title");
			//获取当前的webview
			mainWebview=plus.webview.currentWebview();
           //设置应用在前台运行时系统状态栏的背景颜色，默认使用系统的白色背景
			plus.navigator.setStatusBarBackground("#41cea9")
			//设置只支持竖屏显示
			plus.screen.lockOrientation("portrait-primary");
			//初始化与index有关的页面
			initIndexView();
			
			//检测是否需要显示导航页面todo
			
			//判断是否已经登陆,若没有登陆将预加载登陆页面
			judgelogin();
				//预加载父子模版
			initTemplate();			
		})
		
		
		//初始化
		function  initIndexView (){
			//初始化所有bar页面
			inittabitemWebviews();
			//添加bar点击事件接受
			tapBaritem();
		}
		//初始化所有bar页面
		function inittabitemWebviews(){
			//初始化话的时候让所有的webview预加载并且隐藏
			for (var i = 0; i < barItemArray.length; i++) {
				barItemWebviewArray[i] = mui.preload({
					//设置预加载界面的id
					id:barItemArray[i],
					//设置预加载的界面
					url:barItemArray[i],
					//设置预加载界面的样式
					styles:{
						top:'44px',
						bottom: '51px',
						left: '0px',
						bounce: 'vertical',
						bounceBackground: '#f8f8f8'
					},
					//设置界面是否进行自动加载
					waiting:{
						autoShow:false
					}
				});
//				//初始化的时候所有的界面都隐藏
				barItemWebviewArray[i].hide();
				mainWebview.append(barItemWebviewArray[i]);				
			}
			//初始化的时候第一个界面显示
			barItemWebviewArray[0].show();
			console.log("第一个界面"+barItemWebviewArray[0]);
			
			
			
			//记录当前的显示的webview
			curBarItemWebview = barItemWebviewArray[0];
		}
		
		//添加点击事件
		function tapBaritem(){
			mui('.mui-bar-tab').on('tap','.mui-tab-item',function(){
				var baritem = this;
				//返回 href属性的值
				var baritemurl = baritem.getAttribute('href'); 
//				console.log(baritemurl);
//				console.log(curBarItemWebview.getURL().indexOf(baritemurl));
//				console.log(curBarItemWebview.getURL());
//				console.log(~curBarItemWebview.getURL().indexOf(baritemurl));
//				
//				indexof()如果为false返回－1所以前面加上~，返回的是包含baritemurl内容的第一个索引下标
				if (!~curBarItemWebview.getURL().indexOf(baritemurl)) {
					for (var i = 0; i < barItemArray.length; i++) {
						if (barItemArray[i] == baritemurl) {
							//更改头部名字
							navTitle.innerText = baritem.children[baritem.children.length-1].innerText;
							
							console.log(navTitle.innerText);
							
							//切换baritemwebview
							barItemWebviewArray[i].show();
							curBarItemWebview = barItemWebviewArray[i];
							console.log(barItemWebviewArray[i]);
							break;
						}
					}
				}
			});
		}
			//预加载template
		function initTemplate(){
			var webview =  mui.preload({
				id:'template',
				url:'template.html',
				styles:{
					top: '-1000px',
				}
			});
			webview.show();
		};
		
		function judgelogin() {
			//测试语句
			localStorage.removeItem('user');
			//判断是否已经登录成功//localstorage在页面关闭的时候也同样存在，sessionstorage页面关闭数据不存在
			if (!localStorage.getItem('user')) {
				mui.preload({
					url:'mine/login.html',
					id:'mine/login.html',
					styles:{
						top:'0px',
						bottom:'0px'
					}
				});
			}
		}
		
    </script>
</body>
</html>

出现的错误的原因是barItemArray数组中存储的导航条目和底部导航<nav>标签中<a>标签的href存储的内容不一致造成的
<a class="mui-tab-item" href="barItem/wishList.html">的内容
var barItemArray = ['homepage/home.html','categoryfenlei.html','homepage/wishList.html','homepage/cart.html','homepage/mine.html'];的内容，这是错误的关键，这样写的时候只有分类标签起作用，barItemArray修改成下面的内容就可以了
barItemArray = ['barItem/home.html','categoryfenlei.html','barItem/wishList.html','barItem/cart.html','barItem/mine.html']
barItem是存放导航页面的文件夹



