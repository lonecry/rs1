<template>
    <div class = " box">
        <div class = "loginbox">
            <div class = "ipt">
                <input class = "username " placeholder = "请输入用户名"/>
                <i-icon type = "mine" size = "26" color = "#ACACAC" class = "usericon"/>
            </div>
            <div class = "ipt">
                <input class = "password " type = "password" placeholder = "请输入密码"/>
                <i-icon type = "lock" size = "26" color = "#ACACAC" class = "usericon"/>
            </div>
        </div>
    </div>
</template>
<script>
    export default {
        data(){
            return {}
        },
        components: {},
        methods: {
            bindViewTap(){
            },
            clickHandle(ev){
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
        margin-bottom: 50rpx;

    }
    .ipt input {
        width:88%;
        height:100%;
        margin-left:12%;

    }
    .usericon {
        position  : absolute;
        left      : 5rpx;
        top       : 0rpx;
        font-size : 18rpx;
    }
    .password {
        /*margin-top : 10rpx;*/
    }
</style>
