<template>
    <transition name="fade">
        <div v-if="flag">
            <ul class="wizard" :style="wizardStyle" @touchstart="touchStart" @touchend="touchEnd" ref="wizard">
                <li><img src="../../static/images/q1@2x.png" alt=""></li>
                <li><img src="../../static/images/q2@2x.png" alt=""></li>
                <li style="position: relative">
                    <img src="../../static/images/q3@2x.png" alt="">
                    <div @click="goto" class="btn" >{{enter}}</div>
                </li>
            </ul>
            <div class="circle-navbar" style="display:none">
                <span class="circle-nav" :class="{active: this.index === 0}"></span>
                <span class="circle-nav" :class="{active: this.index === 1}"></span>
                <span class="circle-nav" :class="{active: this.index === 2}"></span>
            </div>
        </div>
    </transition>
</template>

<script>
export default {
    name: "wizardSheet",
    data() {
        return {
            index: 0,
            startX: 0,
            moveX: 0,
            endX: 0,
            enter:"进入应用"
        };
    },
    props: ["flag"],
    mounted(){
        
    },
    computed: {
        wizardStyle() {
            return {
                transform: `translateX(-${this.index * 10.8}rem)`
            };
        }
    },
    methods: {
        touchStart(ev) {
            ev = ev || event;
            if (ev.touches.length === 1) {
                this.startX = ev.touches[0].clientX;
            }
        },
        touchEnd(ev) {
            ev = ev || event;
            let wd = this.$refs.wizard.offsetWidth / 3;
            if (ev.changedTouches.length === 1) {
                this.endX = ev.changedTouches[0].clientX;
                let disX = this.endX - this.startX;
                // 滑动距离大于1/5屏幕宽度时响应切换
                if (Math.abs(disX) > wd / 5) {
                    if (disX < 0 && this.index < 2) {
                        // left slide
                        this.index += 1;
                    } else if (disX > 0 && this.index > 0) {
                        // right slide
                        this.index -= 1;
                    }
                }
            }
        },
        goto() {
            // console.log(111);
            let flag = false;
            this.$emit("setflag", flag);
            this.$router.push({
                path:'/login'
            }); 
        }
    }
};
</script>

<style scoped>
    .wizard{ 
        overflow: hidden;
        z-index: 1000;
        width: 300vw;
        height: 100%;
        position: fixed;
        top: 0;
        left: 0;
        transition: all 0.3s;
    }
    .wizard>li{
        float: left;
        width: 100vw;
        height: 100%;
        background-size: cover;
        background-position: center center; 
        background-repeat: no-repeat;
    }
    .wizard>li img {
        width: 100%;
        height: 100%;
    }
    /* .wizard>li:nth-of-type(1){
        background-image: url('../../static/images/q1@2x.png')
    }
    .wizard>li:nth-of-type(2){
        background-image: url('../../static/images/q2@2x.png')
    }
    .wizard>li:nth-of-type(3){
        background-image: url('../../static/images/q3@2x.png')
    } */
    .circle-navbar{
        z-index: 1001;
        position: absolute;
        width: 100vw;
        bottom: 20px;
        height: 20px;
        display: flex;
        display: -webkit-flex;
        flex-grow: row;
        flex-wrap: nowrap;
        justify-content: center;
        align-items: center;
    }
    .circle-nav{
        width: 8px;
        height: 8px;
        border-radius: 50%;
        border: 1px solid #ccc;
        margin: 0 10px;
    }
    .circle-nav.active{
        background-color: #ccc;
    }
    .btn{
        outline: none;
        border: none;
        border-radius: 0.75rem;
        color: #ffffff;
        background-color: #FFC72E;
        width: 4.63rem;
        height: 1.2rem;
        text-align: center;
        line-height: 1.2rem;
        font-size: 0.48rem;
        position: absolute;
        left: 50%;
        bottom: 15.5%;
        -webkit-transform: translateX(-50%);
        transform: translateX(-50%);
        /* z-index: 99; */
    }
    .fade-leave-active {
        transition: opacity 0.2s;
    }
    .fade-leave-to{
        opacity: 0;
    }
</style>