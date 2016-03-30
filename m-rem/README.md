用Rem来无脑还原Web移动端自适应的页面:

#####代码：  
````
(function (win,doc){
    if (!win.addEventListener) return;
    var html=document.documentElement;
    function setFont()
    {var cliWidth=html.clientWidth;
        html.style.fontSize=100*(cliWidth/640)+'px';    
    }   
    win.addEventListener('resize',setFont,false)
    doc.addEventListener('DOMContentLoaded',setFont,false)
})(window,document);

````
  
#####解读：  
&emsp;&emsp;首先，不去鸟不支持事件监听的浏览器（IE6、7、8），当然不写也可以，因为对字号的自适应的需求都是移动端。  
&emsp;&emsp;当出现窗口大小改变的时候给window绑定一个监听，运行一个叫setFont的函数；当页面的Dom结构加载完也运行setFont（或者不监听直接setFont()运行也可以。）  
&emsp;&emsp;setFont函数用来获取屏幕分辨率，然后按比例来设置html的字号。
&emsp;&emsp;我这里是以100px为基础来缩放。为什么是100px？待我细细说明：  
我们一般拿到手的设计稿都是640px的，我们不以手机分辨率为考量，如果单纯1：1还原640px的页面，那么我们的页面根目录的字号就是`100px（100*（640/640）=100px）`，那么那么设计稿上选择一个文案，然后PS告诉我们字号是24px，那么我们就在页面里给这段文案设置成0.24rem,那么640px的页面上字体就是24px啦。  
&emsp;&emsp;然后当我们考虑比如页面是5/5s上看，那么当前页面字号就是`50px（100*(320/640)=50px）`，那么0.24rem会以12px的大小展示出来。而640px宽的设计稿上的24px的字体，在320px的页面下，就是以12px显示的~  
&emsp;&emsp;这就是为什么要以100px为基础字号，这样页面里量的是24px的字体，我代码里写0.24rem就会自动在页面里以12px显示了~而且它可以在6/6p上以13或者14px的样式展示出来~  
&emsp;&emsp;那你说，我遇到了奇葩的750px为分辨率的设计稿（比如是iphone6的），那你就把公式改成 `100*(cliWidth/750)`+px就行（也就是设计稿是多宽，公式里就写多少）。然后设计稿里量的Xpx大小，就写多少0.Xrem~什么分辨率的设计稿都不怕啦  
#####tips：    
&emsp;&emsp;100px只是为了让我偷懒不用去换算字体大小的，如果喜欢可以自己订1000px，然后写0.012rem这样。但是不要写像10px这样的基础字号，因为有些浏览器有最小字号的限制，比如设置了页面基础字号是10px，但是实际上最小只认11px，那么2rem的字体，本身是希望以20px显示，可能最后是22px显示的。
