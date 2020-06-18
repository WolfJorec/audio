# audio
基于jQ的微信语音样式及功能

### 主要html
```js
<div class='u-flex-bc bg-blue u-w-2/3 mt20 mb20 ml20 pl15 pr15 u-radius-5 audio-btn'>
    <div class="u-posr pt5 pb5">
        <div class="wifi-symbol u-posr">
            <div class="wifi-circle wifi-symbol-first"></div>
            <div class="wifi-circle wifi-symbol-second"></div>
            <div class="wifi-circle wifi-symbol-third"></div>
        </div>
    </div>
    <div class='timer f20 white' id='timer'>04:30</div>

    <audio src="../assets/陈洁仪-喜欢你.mp3" preload="auto" id='myAudio'></audio>
</div>
```
### 主要css
###### 最外层旋转角度，并overflow:hideen才会有左到右wifi效果
```js
.wifi-symbol {
    width: 15px;
    height: 15px;
    box-sizing: border-box;
    overflow: hidden;
    transform: rotate(135deg);
}
.wifi-circle {
    border: 2px solid #fff;
    border-radius: 50%;
    position: absolute;
}

.wifi-symbol-first {
    width: 2px;
    height: 2px;
    background: #cccccc;
    top: 10px;
    left: 10px;
}

.wifi-symbol-second {
    width: 15px;
    height: 15px;
    top: 6px;
    left: 6px;
}

.wifi-symbol-third {
    width: 30px;
    height: 30px;
    top: 0;
    left: 0;
}
```
### 主要js
###### 逻辑很简单，audio标签默认隐藏，点击dom操作audio的事件。
```js
// 获取audio元素
const myAudio = document.getElementById("myAudio");
// 记录开启或关闭的状态
let bool = true;

// 监听元素事件获取音频时间
myAudio.addEventListener("durationchange", function() {
    // 显示播放时长
    $("#timer").html(`${ Math.floor((myAudio.duration / 60) % 60) < 10 ? '0' + Math.floor((myAudio.duration / 60) %
        60) : Math.floor((myAudio.duration / 60) % 60)} : ${ Math.floor(myAudio.duration % 60) < 10 ? '0' + Math.floor(myAudio.duration % 60) : Math.floor(myAudio.duration % 60)}`)

    // 获取时间并计算总秒数
    let startTime = $("#timer").html().split(':')
    // 这里获取倒计时的起始时间
    let SysSecond = +startTime[0] * 60 + +startTime[1];
    // 定时器
    let InterValObj = null 

    $('.audio-btn').on('click', function() {
        // 清除定时器
        if(InterValObj) { window.clearInterval(InterValObj) }

        if(bool) {
            // 开启时，添加开启播放的样式
            $('.wifi-symbol-second').addClass('animation1')
            $('.wifi-symbol-third').addClass('animation2')
        
            // 开启倒计时 并播放
            myAudio.play();
            InterValObj = window.setInterval(SetRemainTime, 1000);
            // 并更改状态
            bool = false
        } else {
            // 关闭时，删除播放的样式
            $('.wifi-symbol-second').removeClass('animation1')
            $('.wifi-symbol-third').removeClass('animation2')

            // 暂停播放
            myAudio.pause();
            // 并更改状态
            bool = true
        }
    })

    //将时间减去1秒，计算天、时、分、秒
    function SetRemainTime() {
        if (SysSecond > 0) {
            SysSecond = SysSecond - 1;
            let second = Math.floor(SysSecond % 60);            // 计算秒     
            let minite = Math.floor((SysSecond / 60) % 60);      //计算分 
            let hour = Math.floor((SysSecond / 3600) % 24);      //计算小时
            let day = Math.floor((SysSecond / 3600) / 24);       //计算天 
            $("#timer").html(`${minite < 10 ? '0' + minite : minite} : ${second < 10 ? '0' + second : second}`);
        } else {
            //剩余时间小于或等于0的时候，就停止间隔函数
            window.clearInterval(InterValObj);
            // 关闭时，删除播放的样式
            $('.wifi-symbol-second').removeClass('animation1')
            $('.wifi-symbol-third').removeClass('animation2')
            // 并更改状态
            bool = true
        }
    }
});
```
