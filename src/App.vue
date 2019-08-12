<script>
    export default {
        created(){
            // 调用API从本地缓存中获取数据
            /*
             * 平台 api 差异的处理方式:  api 方法统一挂载到 mpvue 名称空间, 平台判断通过 mpvuePlatform 特征字符串
             * 微信：mpvue === wx, mpvuePlatform === 'wx'
             * 头条：mpvue === tt, mpvuePlatform === 'tt'
             * 百度：mpvue === swan, mpvuePlatform === 'swan'
             * 支付宝(蚂蚁)：mpvue === my, mpvuePlatform === 'my'
             */
            // let logs
            // if (mpvuePlatform === 'my') {
            //     logs = mpvue.getStorageSync({key: 'logs'}).data || []
            //     logs.unshift(Date.now())
            //     mpvue.setStorageSync({
            //         key: 'logs',
            //         data: logs
            //     })
            // } else {
            //     logs = mpvue.getStorageSync('logs') || []
            //     logs.unshift(Date.now())
            //     mpvue.setStorageSync('logs', logs)
            // }
            // wx.clearStorageSync()
        },
        log(){
            console.log(`log at:${Date.now()}`)
        },
        methods : {
            queryUsreInfo: function(){  //查询用户信息这一步是需要用户授权的
                var that = this
                let uid = wx.getStorageSync("openid")
                wx.getUserInfo({
                    success: function(res){
                        console.log("用户最终授权过")
                        var userInfo = res.userInfo
                        wx.setStorageSync("userInfo",userInfo)
                        wx.request({
                            url    : 'https://hd.xmountguan.com/omf/i_updateuser.aspx?uid=' + uid + '&nickname=' + userInfo.nickName + '&headimg=' + userInfo.avatarUrl,
                            method : 'GET',
                            success: function(rlt){
                                console.log(rlt);
                                wx.setStorageSync("infoSave",true)
                            },
                            fail   : function(e){
                                wx.setStorageSync("infoSave",false)
                            }
                        })
                    }
                })
            },
            getAccess    : function(){   //获取用户信息接口  这个可以直接获取。不需要授权
                var that = this
                that.getOpenid().then((json) =>{

                    if(json.UID){
                        wx.setStorageSync('login','user has already login')
                    }
                    wx.setStorageSync("UID",json.UID)
                    wx.setStorageSync("Mobile",json.Mobile)
                    wx.setStorageSync("openid",json.OpenID)
                }).catch(() =>{
                    wx.showToast({
                        title   : "网络失败请重试！",
                        icon    : 'none',
                        mask    : false,
                        duration: 2000
                    })
                })
            },
            getOpenid    : function(){
                var pms = new Promise((resolve,reject) =>{
                    wx.request({
                            url    : 'https://hd.xmountguan.com/railway/i_getopenid_baoxiu.aspx?code=' + wx.getStorageSync("code"),
                        method : 'GET',
                        success: function(rlt){
                            console.log(rlt.data);
                            var json = rlt.data
                            resolve(json)
                        },fail : function(){
                            console.log('failed')
                            reject("fail")
                        }
                    })
                })
                return pms
            },
            deny         : function(){//拒绝授权
                //用户按了拒绝按钮
                console.log("用户按了拒绝按钮");
                wx.showModal({
                    title      : '温馨提示',
                    content    : '您点击了拒绝授权，将无法进入小程序，请授权之后再进入!',
                    showCancel : false,
                    confirmText: '返回授权',
                    success    : function(res){
                        if(res.confirm){
                            console.log('用户点击了“返回授权”')
                        }
                    }
                })
            },
            checkOpenId  : function(){
                var sessionKey = wx.getStorageSync("openid")
                let check = ((sessionKey) =>{
                    if(sessionKey){
                        var reg = /[A-Za-z]/g
                        var isinit = reg.test(sessionKey) //是否包含字母
                        if(isinit){
                            console.log("包含字母")
                            return false
                        } else {
                            console.log("不包含字母")
                            return true
                        }
                    } else {
                        return false
                    }
                })(sessionKey)
                return check
            },
            checkUserInfo: function(){
                var userInfo = wx.getStorageSync("userInfo")
                let check = ((userInfo) =>{
                    if(userInfo){
                        console.log("userInfo has already in storage & is " + userInfo);
                        return true //have logIn ed
                    } else {
                        return false
                    }
                })(userInfo)
                return check
            }
        },
        mounted(){
            wx.showShareMenu();
        },
        onLaunch: function(){
            if (wx.canIUse('getUpdateManager')) {
                const updateManager = wx.getUpdateManager()
                updateManager.onCheckForUpdate(function (res) {
                    console.log('onCheckForUpdate====', res)
                    // 请求完新版本信息的回调
                    if (res.hasUpdate) {
                        console.log('res.hasUpdate====')
                        updateManager.onUpdateReady(function () {
                            wx.showModal({
                                title: '更新提示',
                                content: '新版本已经准备好，是否重启应用？',
                                success: function (res) {
                                    console.log('success====', res)
                                    // res: {errMsg: "showModal: ok", cancel: false, confirm: true}
                                    if (res.confirm) {
                                        // 新的版本已经下载好，调用 applyUpdate 应用新版本并重启
                                        updateManager.applyUpdate()
                                    }
                                }
                            })
                        })
                        updateManager.onUpdateFailed(function () {
                            // 新的版本下载失败
                            wx.showModal({
                                title: '已经有新版本了哟~',
                                content: '新版本已经上线啦~，请您删除当前小程序，重新搜索打开哟~'
                            })
                        })
                    }
                })
            }


            /* try {
			 wx.removeStorageSync('openid')
			 wx.removeStorageSync('userInfo')
			 } catch (e) {
			 // Do something when catch error
			 }
			 */
            // wx.clearStorage()
            wx.showShareMenu()
            var that = this
            var check = that.checkOpenId()
            if(check){
                const openid = wx.getStorageSync("openid")
                console.log("openid have already in storage & opend is " + openid);
            } else {
                wx.login({
                    success: function(res){
                        // console.log(res);
                        console.log("code :" + res.code);
                        wx.setStorageSync("code",res.code)
                        that.getAccess() //获取openId
                    },fail : function(){
                        wx.showToast({
                            title   : "当前网络请求失败！",
                            icon    : 'none',
                            mask    : false,
                            duration: 2000
                        })
                    }
                })
            }
        },
    }
</script>
<style>
    .container {
        height          : 100%;
        display         : flex;
        flex-direction  : column;
        align-items     : center;
        justify-content : space-between;
        padding         : 200rpx 0;
        box-sizing      : border-box;
    }
    /* this rule will be remove */
    * {
        transition         : width 2s;
        -moz-transition    : width 2s;
        -webkit-transition : width 2s;
        -o-transition      : width 2s;
    }
</style>
