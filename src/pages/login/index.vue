<template>
    <div class = " box">
        <div class = "loginbox">
            <div class = "ipt">
                <input class = "username " v-model = "cell" placeholder = "请输入手机号码"/>
                <i-icon type = "mobilephone" size = "26" color = "#ACACAC" class = "usericon"/>
            </div>
            <div class = "ipt">
                <input class = "password " type = "password" placeholder = "请输入密码"/>
                <i-icon type = "lock" size = "26" color = "#ACACAC" class = "usericon"/>
            </div>
            <button @click = "handleClick" class = "enter">登录</button>
            <p class = "spans">
                <span class = "sp1" @click = "register">注册</span>
                <span class = "sp2" @click = "pswreset">忘记密码？</span>
            </p>
        </div>
    </div>
</template>
<script>
    export default {
        data(){
            return {
                cell: '13858008245'
            }
        },
        components: {},
        methods: {
            register(){
                console.log('You Just Fucked register');
            },
            pswreset(){
                console.log( this.data.cell)
                console.log('You Just Fucked pswreset');
            },
            handleClick(){
                console.log('You Just Fucked me');
                const url = '../logs/main'
                if (mpvuePlatform === 'wx') {
                    mpvue.switchTab({url})
                } else {
                    mpvue.navigateTo({url})
                }
            }
        },
        created(){
            // let app = getApp()
        }
    }
</script>
<style scoped>
    .box {
        display         : flex;
        flex-direction  : column;
        align-items     : center;
        justify-content : center;
        width           : 100%;
        height          : 100vh;
        /*background      : #acacac;*/
    }
    .row {
        width   : 100%;
        height  : 500rpx;
        display : block;
    }
    .loginbox {
        width      : 500rpx;
        /*background : rgba(33, 191, 255, 0.18);*/
        height     : 700rpx;
        text-align : center;
        /*border     : 1rpx solid gray;*/
        position   : relative;
    }
    .ipt {
        width         : 100%;
        height        : 60rpx;
        line-height   : 60rpx;
        font-size     : 19rpx;
        background    : white;
        border-radius : 18rpx;
        text-align    : left;
        border        : 1rpx solid #b1b1b1;
        position      : relative;
        margin-bottom : 50rpx;

    }
    .ipt input {
        width       : 88%;
        height      : 100%;
        margin-left : 12%;

    }
    .usericon {
        position  : absolute;
        left      : 5rpx;
        top       : -1rpx;
        font-size : 18rpx;
    }
    .password {
        /*margin-top : 10rpx;*/
    }
    .enter {
        width       : 100%;
        /*height: 50rpx;*/
        background  : #2d8cf0;
        position    : relative;
        color       : white;
        margin-top  : 115rpx;
        height      : 72rpx;
        line-height : 72rpx;
    }
    .spans {
        width      : 100%;
        height     : 10rpx;
        font-size  : 20rpx;
        margin-top : 5rpx;
    }
    .sp1 {
        float : left;
        color : #2d8cf0;
    }
    .sp2 {
        float : right;
    }
</style>
