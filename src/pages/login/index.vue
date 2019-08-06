<template>
    <div class=" box">
        <h1 class="miantitle">登录</h1>
        <div class="loginbox">
            <div class="ipt">
                <input class="username " v-model="cell" placeholder="请输入手机号码"/>
                <i-icon type="mobilephone" size="26" color="#ACACAC" class="usericon"/>
            </div>
            <div class="ipt">
                <input class="password " v-model="psw" type="password" placeholder="请输入密码"/>
                <i-icon type="lock" size="26" color="#ACACAC" class="usericon"/>
            </div>
            <button @click="handleClick" class="enter">登录</button>
            <p class="spans">
                <span class="sp1" @click="register">注册</span>
                <span class="sp2" @click="pswreset">忘记密码？</span>
            </p>
        </div>
        <i-toast id="toast"/>
    </div>
</template>
<script>
    import {$Toast} from '../../../static/iview/base/index'

    export default {
        data() {
            return {
                cell: '13111111111',
                psw: '12345678'
            }
        },
        components: {},
        methods: {
            register() {
                console.log('You Just Fucked register');
                // mpvue.setStorageSync({
                //     "cell": this.cell?this.cell:"111",
                //     "psw": this.psw?this.psw:"sadasd"
                // })
                mpvue.navigateTo({
                    url: '../register/main',
                })
            },
            pswreset() {
                console.log('You Just Fucked pswreset');
                mpvue.navigateTo({
                    url: '../changecell2/main',
                })
            },
            handleClick() {
                console.log(this.cell)
                var wx = mpvue;

                wx.request({
                    url: 'https://hd.xmountguan.com/railway/user.aspx?func=login&mobile=' + this.cell + '&pwd=' + this.psw + '&role=1', //仅为示例，并非真实的接口地址

                    success(res) {
                        console.log(res.data)
                        wx.setStorageSync("UID",res.data.UID)
                        if (res.data.UID) {
                            const url = '../indexswiper/main'
                            if (mpvuePlatform === 'wx') {
                                console.log('mpvue switchTab ');
                                // mpvue.switchTab({url})

                                mpvue.setStorageSync('login', 'user has already login')
                                mpvue.navigateTo({url})
                            } else {
                                console.log('mpvue navigateTo ');
                                // mpvue.navigateTo({url})
                            }
                        } else {
                            $Toast({
                                content: '账号或者密码错误',
                                type: 'warning'
                            });
                        }
                    }
                })


            },
            handleWarning() {
                $Toast({
                    content: '警告的提示',
                    type: 'warning'
                });
            },
        },
        created() {
            // let app = getApp()
        },
        onShow() {
            wx.showShareMenu();
        }
    }
</script>
<style scoped>
    .miantitle {
        width: 100%;
        display: block;
        text-align: center;
        font-size: 35rpx;
        position: absolute;
        top: 22rpx;
        left: 0;

    }

    .box {
        display: flex;
        flex-direction: column;
        align-items: center;
        justify-content: center;
        width: 100%;
        height: 100vh;
        /*background      : #acacac;*/
    }

    .row {
        width: 100%;
        height: 500rpx;
        display: block;
    }

    .loginbox {
        width: 500rpx;
        height: 540rpx;
        text-align: center;
        position: relative;

    }

    .ipt {
        width: 100%;
        height: 60rpx;
        line-height: 60rpx;
        font-size: 22rpx;
        background: white;
        border-radius: 18rpx;
        text-align: left;
        border: 1rpx solid #b1b1b1;
        position: relative;
        margin-bottom: 50rpx;

    }

    .ipt input {
        width: 88%;
        height: 100%;
        margin-left: 12%;

    }

    .usericon {
        position: absolute;
        left: 5rpx;
        top: -1rpx;
        font-size: 18rpx;
    }

    .password {
        /*margin-top : 10rpx;*/
    }

    .enter {
        width: 100%;
        /*height: 50rpx;*/
        background: #2d8cf0;
        position: relative;
        color: white;
        margin-top: 115rpx;
        height: 72rpx;
        line-height: 72rpx;
        font-size: 28rpx;
    }

    .spans {
        width: 100%;
        height: 10rpx;
        font-size: 20rpx;
        margin-top: 25rpx;
    }

    .sp1 {
        float: left;
        color: #2d8cf0;
    }

    .sp2 {
        float: right;
    }
</style>
