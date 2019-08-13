<template>
    <div class = " box">
        <h1 class = "miantitle">重置密码</h1>
        <div class = "loginbox">
            <div class = "ipt">
                <input class = "usercell" v-model = "cell"   placeholder = "请输入手机号码"/>
                <i-icon type = "mobilephone" size = "26" color = "#ACACAC" class = "usericon"/>
            </div>
            <div class = "ipt">
                <input class = "password " v-model = "psw" type = "password" placeholder = "新密码"/>
                <i-icon type = "lock" size = "26" color = "#ACACAC" class = "usericon"/>
            </div>
            <div class = "ipt">
                <input class = "password " v-model = "psw2" type = "password" placeholder = "确认新密码"/>
                <i-icon type = "lock" size = "26" color = "#ACACAC" class = "usericon"/>
            </div>
            <div class = "ipt">
                <input class = "password " v-model = "authcode" placeholder = "验证码" style = "width:50%;"/>
                <i-icon type = "lock" size = "26" color = "#ACACAC" class = "usericon"/>
                <button @click = "getcode" class = "getcode" :disabled = "btnable">{{codeText}}</button>
            </div>
            <button @click = "zhuce" class = "enter">重置</button>
        </div>
        <i-toast id = "toast"/>
    </div>
</template>
<script>
    import {$Toast} from '../../../static/iview/base/index'

    export default {
        data(){
            return {
                btnable : "",
                codeText: "获取验证码",
                count   : '',
                name    : '张三',
                cell    : '',
                psw     : '',
                psw2    : '',
                vcode   : "",
                timer   : "",
                authcode: ''
            }
        },
        components: {},
        methods   : {
            zhuce(){
                var _this = this
                console.log('You Just Fucked register');
                mpvue.setStorageSync("Mobile",this.cell)
                // mpvue.setStorageSync("psw",this.psw)
                if(this.check()){
                    //提交重置
                    wx.request({
                        url: "https://hd.xmountguan.com/railway/user.aspx?func=resetPwd&mobile=" + _this.cell + "&newpwd=" + _this.psw + '&vcode=' + _this.authcode,
                        success(res){
                            console.log(res.data)
                            if(res.data.result == "1|reset success"){
                                $Toast({
                                    content : '重置成功',
                                    type    : 'success',
                                    duration: 2,
                                });
                                wx.redirectTo({
                                    url: '../login/main'
                                })
                            }
                        },
                        fail(){
                            console.log('网络错误')
                            $Toast({
                                content: '网络错误，请稍后重试',
                                type   : 'warning'
                            });
                        }
                    })
                }
            },
            getcode(){
                var _this = this
                if(this.cell == ""){
                    $Toast({
                        content: '请输入手机号码',
                        type   : 'warning'
                    });
                } else {

                    //提交
                    wx.request({
                        url: 'https://hd.xmountguan.com/railway/other.aspx?func=SendSms&mobile=' + _this.cell,
                        success(res){
                            if(res.data.success = "success"){
                                $Toast({
                                    content: '验证码已发送',
                                    type   : 'warning'
                                });
                                // this.disabled = "disabled"
                                _this.btnable = "disabled";
                                console.log('You Just Fucked getcode');
                                const TIME_COUNT = 15;
                                if(! _this.timer){
                                    _this.count = TIME_COUNT;
                                    _this.timer = setInterval(() =>{
                                        if(_this.count > 0 && _this.count <= TIME_COUNT){
                                            _this.count --;
                                            _this.codeText = _this.count + 's后重新获取'
                                        } else {
                                            _this.btnable = "",
                                                clearInterval(_this.timer);
                                            _this.codeText = "获取验证码"
                                            _this.timer = null;
                                        }
                                    },1000)
                                }
                            }
                        },
                        fail(){
                            console.log('网络错误')
                            $Toast({
                                content: '网络错误，请稍后重试',
                                type   : 'warning'
                            });
                        }
                    })
                }
            }
            ,
            check(){
                if(this.cell == ""){
                    $Toast({
                        content: '请输入正确的手机号',
                        type   : 'warning'
                    });
                    return false
                } else if(! (/^1[3456789]\d{9}$/.test(this.cell))){
                    $Toast({
                        content: '手机号码有误，请重填',
                        type   : 'warning'
                    });
                    return false
                } else if(this.psw == ""){
                    $Toast({
                        content: '请输入新密码',
                        type   : 'warning'
                    });
                    return false
                } else if(this.psw2 == ""){
                    $Toast({
                        content: '请再次输入新密码',
                        type   : 'warning'
                    });
                    return false
                } else if(this.psw2 != this.psw){
                    $Toast({
                        content: '两次密码不一致',
                        type   : 'warning'
                    });
                    return false
                } else if(this.authcode == ""){
                    $Toast({
                        content: '请获取验证码',
                        type   : 'warning'
                    });
                    return false
                } else {
                    return true
                }
            }
        },
        mounted(){
            this.cell = wx.getStorageSync("Mobile")
            this.psw='';
            this.psw2='';
            this.authcode='';
        }

    }
</script>
<style scoped>
    .miantitle {
        width      : 100%;
        display    : block;
        text-align : center;
        font-size  : 35rpx;
        position   : absolute;
        top        : 22rpx;
        left       : 0;
    }
    .getcode {
        width       : 185rpx;
        height      : 54rpx;
        display     : block;
        float       : right;
        position    : absolute;
        top         : 4rpx;
        right       : 10rpx;
        background  : #2d8cf0;
        line-height : 54rpx;
        color       : white;
        font-size   : 26rpx;
        padding     : 0;
        box-sizing  : border-box;
    }
    .box {
        display         : flex;
        flex-direction  : column;
        align-items     : center;
        justify-content : center;
        width           : 100%;
        height          : 100vh;
        /*background      : #33ac59;*/
        overflow        : hidden;
    }
    .row {
        width   : 100%;
        height  : 500rpx;
        display : block;
    }
    .loginbox {
        width       : 500rpx;
        height      : 709rpx;
        text-align  : center;
        position    : relative;
        padding-top : 20rpx;
    }
    .ipt {
        width         : 100%;
        height        : 60rpx;
        line-height   : 60rpx;
        font-size     : 22rpx;
        background    : white;
        border-radius : 18rpx;
        text-align    : left;
        border        : 2rpx solid #b1b1b1;
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
        font-size   : 28rpx;
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
    .i-btn {
        margin : 0;
    }
</style>
