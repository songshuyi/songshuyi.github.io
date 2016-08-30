http://songshuyi.github.io/pay_img/    //红包照片
http://songshuyi.github.io/music/      //音乐播放器

new命令的原理
1.创建一个空对象，作为将要返回的对象实例
2.将这个空对象的原型，指向构造函数的prototype属性
3.将这个空对象赋值给函数内部的this关键字
4.开始执行构造函数内部的代码

progress样式
.progress::-webkit-progress-bar { background: #ddd;}
.progress::-webkit-progress-value {
   background: -webkit-gradient(linear,0% 50%,100% 50%,from(#ffe04d),
   color-stop(0.25,#ffc33a),color-stop(0.5,#ff863a),color-stop(0.75,#ff6d3a),to(#ff3a48));
}

0.5像素边框
.half_border{position:relative;}
.half-border:before{
  content: '';box-sizing: border-box;
  position: absolute;top:0;left: 0;
  border: 1px solid #000;
  width: 200%;height: 200%;
  -webkit-transform: scale(0.5);
  -webkit-transform-origin:  0 0;
}

微信分享
<script type="text/javascript" src="http://res.wx.qq.com/open/js/jweixin-1.0.0.js">
wx.config({
    debug: false, // 开启调试模式,调用的所有api的返回值会在客户端alert出来
    appId: '<?php echo $config['appid'];?>', // 必填，公众号的唯一标识
    timestamp: <?php echo $config['timestamp'];?>, // 必填，生成签名的时间戳
    nonceStr: '<?php echo $config['noncestr'];?>', // 必填，生成签名的随机串
    signature: '<?php echo $config['signature'];?>',// 必填，签名，见附录1
    jsApiList: ['onMenuShareTimeline', 'onMenuShareAppMessage'] // 必填，需要使用的JS接口列表
});
wx.ready(function () {
    //分享到朋友圈
    wx.onMenuShareTimeline({
        title: '', // 分享标题
        link: '', // 分享链接
        imgUrl: '', // 分享图标
        success: function () {
        },
        cancel: function () {
        }
    });
    //分享给朋友
    wx.onMenuShareAppMessage({
        title: '', // 分享标题
        desc: '',//描述
        link: '', // 分享链接
        imgUrl: '', // 分享图标
        type: 'link', // 分享类型,music、video或link，不填默认为link
        success: function () {
        },
        cancel: function () {
        }
    });
});

h5与native交互
function app_js(key, data) {
    var ua = navigator.userAgent.toLowerCase();
    if (/iphone|ipad|ipod/.test(ua)) {
        var url = "local://" + key + "?data=" + encodeURIComponent(JSON.stringify(data));
        location.href = url;
    } else if (/android/.test(ua)) {
        controller[key](JSON.stringify(data));
    }
}

原生js的ajax请求
function post(url,param,callback){
    var xmlhttp= new XMLHttpRequest();
    var paraStr = '';
    xmlhttp.onreadystatechange=function(){
        if(this.readyState==4 && this.status == 200){
            callback(JSON.parse(xmlhttp.responseText));
        }
    };
    xmlhttp.open('post',url,true);
    xmlhttp.setRequestHeader("Content-type","application/x-www-form-urlencoded");
    xmlhttp.setRequestHeader("If-Modified-Since", "0");
    for (key in param){
        paraStr += key+"="+encodeURIComponent(param[key])+"&"
    }
    paraStr = paraStr.substr(0,paraStr.length-1);
    xmlhttp.send(paraStr);
}

图片延迟加载
<script src="echo.min.js"></script>
echo.init({
    offset: 0,
    throttle: 0
});

轮播图
<script src="TouchSlide.1.1.js"></script>
TouchSlide({
    slideCell:"#slideBox",
    titCell:".hd ul", //开启自动分页 autoPage:true ，此时设置 titCell 为导航元素包裹层
    mainCell:".bd ul",
    effect:"left",
    autoPage:true,//自动分页
    autoPlay:false, //自动播放
    interTime:3500
});

h5打开app
<script src="http://a.mlinks.cc/scripts/dist/mlink.min.js"></script>
var btn = document.querySelector("a#like");
new Mlink([
    {
        mlink: "http://a.mlinks.cc/AA1F?id=<?php echo $gid?>", // 在魔窗后台配置好的短链URL
        button: btn //必须是A标签
    }
]);

倒计时
function getTime(end_time,now_time,callback){
    var time = Math.abs(new Date(Date.parse(end_time.replace(/-/g, "/"))).getTime()
     - new Date(Date.parse(now_time.replace(/-/g, "/"))).getTime())/1000;
    var second = Math.floor(time%60);
    var minute = Math.floor(time/60%60);
    var hour = Math.floor(time/3600%24);
    var day = Math.floor(time/3600/24);
    setInterval(function(){
        if(second !=0){
            second --;
        }else if(second == 0 && (minute > 0 || hour > 0 || day > 0)){
            second = 59;
            if(minute == 0 && (hour > 0 || day > 0)){
                minute = 59;
                if(hour == 0 && day > 0){
                    hour = 23;
                    day --;
                }else{
                    hour --;
                }
            }else{
                minute --;
            }
        }else{
            window.location.reload();
        }
        callback(day,hour,minute,second);
    },1000);
}