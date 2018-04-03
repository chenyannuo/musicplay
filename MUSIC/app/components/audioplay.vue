<style scoped lang='stylus'>
.player {
    width: 100%;
    background: #fff;
    position: fixed;
    top: 0;
    left: 0;
    z-index: 1;
    header {
        height: 50px;
        line-height: 50px;
        text-align: center;
        font-size: 16px;
        color: #333;
        position: relative;
        /* background: #c20c0c; */
        span {
            width: 50px;
            height: 50px;
            text-align: center;
            line-height: 50px;
            font-size: 24px;
            color: #000;
            display: block;
            position: absolute;
            top: 0;
            left: 0;
        }
    }
    .content_cd {
        width: 100%;
        padding-top: 50px;
        position: relative;
        overflow: hidden;
        .cd {
            width: 100%;
            position: relative;
            animation: rotate 5s linear infinite paused;
            img.imgCd {
                width: 70%;
                margin: 0 auto;
                display: block;
                border-radius: 9999px;
                box-shadow: 0px 0px 30px #fff;
                border: 10px solid rgba(255, 255, 255, 0.6);
            }
            .singerImg {
                width: 40%;
                position: absolute;
                top: 50%;
                left: 50%;

                transform: translate(-50%, -50%);
                overflow: hidden;
                border-radius: 50%;
                img {
                    width: 100%;
                    display: block;
                }
            }
        }
        .cd.cur {
            animation-play-state: running;
        }
        @keyframes rotate {
            from {
                transform: rotate(0deg);
            }
            to {
                transform: rotate(360deg);
            }
        }
        .swith {
            width: 110px;
            height: 165px;
            position: absolute;
            top: -18px;
            left: 62%;
            transform: translateX(-50%) rotate(-23deg);
            /*这个属性 调整 rotate 圆心点*/
            transform-origin: 15px 15px;
            transition: 1s ease all 0s;
            z-index: 5;
            img {
                width: 100%;
            }
        }
        .swith.cur {
            transform: translateX(-50%) rotate(0deg);
        }
    }
    .setting {
        padding-top: 50px;
        ul {
            overflow: hidden;
            margin: 0 auto;
            width: 70%;
            li {
                float: left;
                width: 25%;
                height: 50px;
                text-align: center;
                line-height: 50px;
                font-size: 24px;
                color: #000;
            }
        }
    }
    .timeBar {
        width: 100%;
        height: 40px;
        .startTime {
            width: 15%;
            height: 40px;
            line-height: 40px;
            font-size: 1px;
            text-align: right;
            color: rgba(255, 255, 255, 0.8);
        }
        .all_duration {
            width: 60%;
            height: 2px;
            margin: 19px 5%;
            span.duration {
                position: relative;
                height: 2px;
                width: 100%;
                background: red;
                display: block;
                span.currentTime {
                    position: absolute;
                    top: 0;
                    left: 0;
                    height: 2px;
                    width: 20%;
                    background: green;
                    display: block;
                }
            }
        }
        .endTime {
            width: 15%;
            height: 40px;
            line-height: 40px;
            font-size: 1px;
            text-align: left;
            color: rgba(255, 255, 255, 0.8);
        }
    }
    .controller {
        ul {
            overflow: hidden;
            li {
                float: left;
                width: 20%;
                height: 50px;
                text-align: center;
                line-height: 50px;
                font-size: 30px;
                color: rgba(255, 255, 255, 0.7);
            }
            li.icon-bofang {
                font-size: 48px;
            }
        }
    }
}

</style>
<template>
    <div class="player" v-height :style="{background :'#223950 url('+ player.playerBg +') 0 0 no-repeat'}">
        <span v-if="typeof player.index == 'number'">	
			<audio id="audio" :src="player.album[player.index].musicUrl" autoplay @timeupdate="play()"></audio> 
		</span>
        <header>
            播放器
            <span @click="goBack" class="iconfont icon-houtui1"></span>
        </header>
        <div class="content_cd">
            <div class="swith" :class="{cur :addSwithClass}">
                <img src="../../resource/img/swith.png">
            </div>
            <div class="cd" :class="{cur :addSwithClass}">
                <img src="../../resource/img/cd.png" class="imgCd">
                <div class="singerImg">
                    <img :src="player.albumImgSrc" alt="">
                </div>
            </div>
        </div>
        <div class="setting">
            <ul>
                <li class="iconfont icon-xin1"></li>
                <li class="iconfont icon-xiazai1"></li>
                <li class="iconfont icon-pinglun"></li>
                <li class="iconfont icon-more-vert"></li>
            </ul>
        </div>
        <div class="timeBar ovh">
            <p class="startTime fl">{{currentTime | zhuanhuan}}</p>
            <p class="all_duration fl">
                <span class="duration">
					<span class="currentTime"></span>
                </span>
            </p>
            <p class="endTime fl">{{duration | zhuanhuan}}</p>
        </div>
        <div class="controller">
            <ul>
                <li class="iconfont icon-renwu_xh"></li>
                <li class="iconfont icon-previous"></li>
                <li class="iconfont icon-bofang" @click="play_pause"></li>
                <li class="iconfont icon-next"></li>
                <li class="iconfont icon-icon8"></li>
            </ul>
        </div>
        <!-- <div>
			<ul>
				<li v-for="item in player.album">{{item.musicName}}</li>
			</ul>
		</div> -->
    </div>
</template>
<script>
export default {
    props: ["player"],
    data() {
        return {
            currentTime: 0,
            duration: 0,
            // 添加旋转 和 磁头
            addSwithClass: false,
            isAddSwithClass: true
        }
    },
    computed: {

    },
    filters: {
        zhuanhuan(s) {
            var t;
            if (s > -1) {
                var min = Math.floor(s / 60) % 60;
                var sec = s % 60;
                if (min < 10) { t += "0"; }
                t = min + ":";
                if (sec < 10) { t += "0"; }
                t += ~~sec;
            }
            return t;
        }
    },
    methods: {
        play_pause() {
            var paused = document.getElementById("audio").paused
            if (!paused) {
                document.getElementById("audio").pause();
                this.isAddSwithClass = false
                this.addSwithClass = false
            } else {
                document.getElementById("audio").play();
                this.isAddSwithClass = true
                this.addSwithClass = true
            }
        },
        goBack() {
            this.$store.commit("SHOWORHIDEPLAYER", {
                isShowPlayer: false
            })
        },
        play() {
            var vm = this;
            var currentTime = document.getElementById("audio").currentTime
            var duration = document.getElementById("audio").duration
            var paused = document.getElementById("audio").paused
            if (!paused) {
                if (this.isAddSwithClass) {
                    this.isAddSwithClass = false
                    this.addSwithClass = true
                }
            }
            vm.currentTime = currentTime;
            vm.duration = duration;
        }
    }
}

</script>
