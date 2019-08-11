<template>
    <div class = " box">
        <!--<button open-type = "getUserInfo" bindgetuserinfo = "goDetail" class = "cytl tlin" style="width: 100rpx;background: red;height: 100rpx;position: fixed;z-index: 1000;">
            <i-icon type="mine" @click="toggleLeft" size="26" color="#ACACAC" class="icon"/>
        </button>-->
        <button open-type = "getUserInfo" @getuserinfo = "bindGetUserInfo" @click = "getUserInfoClick" style = "width: 100rpx;background: transparent;height: 100rpx;position: fixed;z-index: 2;border: none">
            <i-icon type = "mine" size = "26" color = "#ACACAC" class = "icon"/>
        </button>
        <i-tabs class = 'itabs' :current = "current_scroll" @change = "handleChangeScroll">
            <i-tab key = "0" class = "tabs" :title = "'全部'"></i-tab>
            <i-tab key = "1" class = "tabs" :title = "'待处理'"></i-tab>
            <i-tab key = "2" class = "tabs" :title = "'维修中'"></i-tab>
            <i-tab key = "3" class = "tabs" :title = "'已完成'"></i-tab>
            <i-tab key = "4" class = "tabs" :title = "'已中止'"></i-tab>
        </i-tabs>
        <swiper class = "swiper" duration = "300" :current = "current_scroll" @change = "change">
            <swiper-item class = "swiperitem">
                <span  v-if="refreshicon" style =
                    "position:absolute;width:750rpx;left:0;text-align:center;display:block;font-size:20rpx">  下拉刷新<i-icon  size = '14'  type = "refresh"/></span>
                <scroll-view scroll-y = "true" :style = "computedClassObject" @scrolltolower = "loadmore"
                             :upper-threshold = "150" @scrolltoupper = "refresh">
                    <div class = "card" :data-cardid = "item.listId" ref = "dataNum" @click = "cardClick($event)" v-for = "(item,index) in  lists" :key = "index">
                        <span class = "danhao">报修单号:{{item.listNumber}}</span>
                        <span :class = "[{  gray  : item.listState==='0' },{  yellow  : item.listState==='1' },{  green  : item.listState==='2' },{  red  : item.listState==='3' }, 'state']">{{item.listState==0?"待处理":(item.listState==1?"维修中":(item.listState==2?"已完成":"已中止"))}}</span>
                        <div class = "listtime">
                            <i-icon type = "time" size = "29" color = "#ACACAC" class = "listicon"/>
                            <span class = "tdetail">{{item.listTime}}</span>
                        </div>
                        <div class = "listtype">
                            <i-icon type = "setup_fill" size = "29" color = "#ACACAC" class = "listicon"/>
                            <span class = "tdetail">{{item.listType}}</span>
                        </div>
                        <div class = "listlocation">
                            <i-icon type = "coordinates_fill" size = "29" color = "#ACACAC" class = "listicon"/>
                            <span class = "tdetail">{{item.listLoca}}</span>
                        </div>
                    </div>
                </scroll-view>
            </swiper-item>
            <swiper-item class = "swiperitem">
                <span v-if="refreshicon" style = "position:absolute;width:750rpx;left:0;text-align:center;display:block;font-size:20rpx">  下拉刷新<i-icon size = '14'  type = "refresh"/></span>

                <scroll-view :scroll-y = "scrolly" :style = "computedClassObject" @scrolltolower = "loadmore"
                             :upper-threshold = "150" @scrolltoupper = "refresh">
                    <div class = "card" :data-cardid = "item.listId" ref = "dataNum" @click = "cardClick($event)" v-for = "(item,index) in  lists" :key = "index" v-if = "item.listState==0">
                        <span class = "danhao">报修单号:{{item.listNumber}}</span>
                        <span :class = "[{  gray  : item.listState==='0' },{  yellow  : item.listState==='1' },{  green  : item.listState==='2' },{  red  : item.listState==='3' }, 'state']">{{item.listState==0?"待处理":(item.listState==1?"维修中":(item.listState==2?"已完成":"已中止"))}}</span>
                        <div class = "listtime">
                            <i-icon type = "time" size = "29" color = "#ACACAC" class = "listicon"/>
                            <span class = "tdetail">{{item.listTime}}</span>
                        </div>
                        <div class = "listtype">
                            <i-icon type = "setup_fill" size = "29" color = "#ACACAC" class = "listicon"/>
                            <span class = "tdetail">{{item.listType}}</span>
                        </div>
                        <div class = "listlocation">
                            <i-icon type = "coordinates_fill" size = "29" color = "#ACACAC" class = "listicon"/>
                            <span class = "tdetail">{{item.listLoca}}</span>
                        </div>
                    </div>
                </scroll-view>
            </swiper-item>
            <swiper-item class = "swiperitem">
                <span v-if="refreshicon" style = "position:absolute;width:750rpx;left:0;text-align:center;display:block;font-size:20rpx">  下拉刷新<i-icon   size = '14'  type = "refresh"/></span>

                <scroll-view :scroll-y = "scrolly" :style = "computedClassObject" @scrolltolower = "loadmore"
                             :upper-threshold = "150" @scrolltoupper = "refresh">
                    <div class = "card" :data-cardid = "item.listId" ref = "dataNum" @click = "cardClick($event)" v-for = "(item,index) in  lists" :key = "index" v-if = "item.listState==1">
                        <span class = "danhao">报修单号:{{item.listNumber}}</span>
                        <span :class = "[{  gray  : item.listState==='0' },{  yellow  : item.listState==='1' },{  green  : item.listState==='2' },{  red  : item.listState==='3' }, 'state']">{{item.listState==0?"待处理":(item.listState==1?"维修中":(item.listState==2?"已完成":"已中止"))}}</span>
                        <div class = "listtime">
                            <i-icon type = "time" size = "29" color = "#ACACAC" class = "listicon"/>
                            <span class = "tdetail">{{item.listTime}}</span>
                        </div>
                        <div class = "listtype">
                            <i-icon type = "setup_fill" size = "29" color = "#ACACAC" class = "listicon"/>
                            <span class = "tdetail">{{item.listType}}</span>
                        </div>
                        <div class = "listlocation">
                            <i-icon type = "coordinates_fill" size = "29" color = "#ACACAC" class = "listicon"/>
                            <span class = "tdetail">{{item.listLoca}}</span>
                        </div>
                    </div>
                </scroll-view>
            </swiper-item>
            <swiper-item class = "swiperitem">
                <span  v-if="refreshicon" style =
                    "position:absolute;width:750rpx;left:0;text-align:center;display:block;font-size:20rpx">  下拉刷新<i-icon size = '14'  type = "refresh"/></span>

                <scroll-view :scroll-y = "scrolly" :style = "computedClassObject" @scrolltolower = "loadmore"
                             :upper-threshold = "150" @scrolltoupper = "refresh">
                    <div class = "card" :data-cardid = "item.listId" ref = "dataNum" @click = "cardClick($event)" v-for = "(item,index) in  lists" :key = "index" v-if = "item.listState==2">
                        <span class = "danhao">报修单号:{{item.listNumber}}</span>
                        <span :class = "[{  gray  : item.listState==='0' },{  yellow  : item.listState==='1' },{  green  : item.listState==='2' },{  red  : item.listState==='3' }, 'state']">{{item.listState==0?"待处理":(item.listState==1?"维修中":(item.listState==2?"已完成":"已中止"))}}</span>
                        <div class = "listtime">
                            <i-icon type = "time" size = "29" color = "#ACACAC" class = "listicon"/>
                            <span class = "tdetail">{{item.listTime}}</span>
                        </div>
                        <div class = "listtype">
                            <i-icon type = "setup_fill" size = "29" color = "#ACACAC" class = "listicon"/>
                            <span class = "tdetail">{{item.listType}}</span>
                        </div>
                        <div class = "listlocation">
                            <i-icon type = "coordinates_fill" size = "29" color = "#ACACAC" class = "listicon"/>
                            <span class = "tdetail">{{item.listLoca}}</span>
                        </div>
                    </div>
                </scroll-view>
            </swiper-item>
            <swiper-item class = "swiperitem">
                <span  v-if="refreshicon" style =
                    "position:absolute;width:750rpx;left:0;text-align:center;display:block;font-size:20rpx">  下拉刷新<i-icon   size = '14'  type = "refresh"/></span>

                <scroll-view :scroll-y = "scrolly" :style = "computedClassObject" @scrolltolower = "loadmore"
                             :upper-threshold = "150" @scrolltoupper = "refresh">
                    <div class = "card" :data-cardid = "item.listId" ref = "dataNum" @click = "cardClick($event)" v-for = "(item,index) in  lists" :key = "index" v-if = "item.listState==3">
                        <span class = "danhao">报修单号:{{item.listNumber}}</span>
                        <span :class = "[{  gray  : item.listState==='0' },{  yellow  : item.listState==='1' },{  green  : item.listState==='2' },{  red  : item.listState==='3' }, 'state']">{{item.listState==0?"待处理":(item.listState==1?"维修中":(item.listState==2?"已完成":"已中止"))}}</span>
                        <div class = "listtime">
                            <i-icon type = "time" size = "29" color = "#ACACAC" class = "listicon"/>
                            <span class = "tdetail">{{item.listTime}}</span>
                        </div>
                        <div class = "listtype">
                            <i-icon type = "setup_fill" size = "29" color = "#ACACAC" class = "listicon"/>
                            <span class = "tdetail">{{item.listType}}</span>
                        </div>
                        <div class = "listlocation">
                            <i-icon type = "coordinates_fill" size = "29" color = "#ACACAC" class = "listicon"/>
                            <span class = "tdetail">{{item.listLoca}}</span>
                        </div>
                    </div>
                </scroll-view>
            </swiper-item>
        </swiper>
        <i-button @click = "goreport" class = "goreport" type = "primary">一键报修</i-button>
        <i-drawer mode = "left" @click = "toggleLeft" :visible = "showleft" class = "i-drawer-mask">
            <div class = "demo-container" @click.stop>
                <img :src = "userIcon" class = "usericon" @click = "changeIcon" alt = ""/>
                <span class = "username">{{usserName}}</span>
                <view class = "cz chongzhiPsw" @click = "pswreset">
                    <i-icon type = "lock_fill" size = "21" color = "#ACACAC"/>
                    重置密码
                </view>
                <view class = "cz chongzhiCell" @click = "changecell">
                    <i-icon type = "mobilephone_fill" size = "21" color = "#ACACAC"/>
                    更换绑定手机号
                </view>
                <view class = "cz chongzhiCell" @click = "logout">
                    <i-icon type = "offline_fill" size = "21" color = "#ACACAC"/>
                    退出
                </view>
            </div>
        </i-drawer>
    </div>
</template>
<script>
    export default {
        data(){
            return {
                showleft      : false,
                current_scroll: '0',
                userIcon      : '/static/images/Avator.png',
                usserName     : "一万年朝夕",
                scrolly       : true,
                scrollData    : {
                    height                      : '1100rpx',
                    "-webkit-overflow-scrolling": "touch",
                },
                lists         : [],
                page          : '1',
                end           : false,
                refreshicon   : false
            }
        },
        components: {},
        computed  : {
            classObject: function(){
                return {
                    active: this.isActive,
                    sort  : this.isSort,
                }
            },
            computedClassObject(){
                return this.showJson({
                    height: this.scrollData.height,
                })
            }
        },
        methods   : {
            showJson(style){
                let s = []
                for(let i in style){
                    s.push(i + ':' + style[i]);
                }
                s = s.join(';')
                return s
            },
            toggleLeft(e){
                this.showleft = ! this.showleft
            },
            handleChangeScroll(e){
                this.current_scroll = e.target.key;
            },
            changeIcon(e){
                console.log(e);
            },
            handleState(state){
                // 待处理 0 灰色
                // 维修中 1 黄色
                // 已完成 2  绿色
                // 已中止 3  红色
                switch(state){
                    case 0:
                        return "待处理";
                        break;
                    case 1:
                        return "维修中";
                        break;
                    case 2:
                        return "待处理";
                        break;
                    case 3:
                        return "已完成";
                        break;
                }
            },
            cardClick(e){
                // console.log(e.currentTarget.dataset.cardid);
                // console.log(e.target);
                console.log("cardid is " + e.mp.currentTarget.dataset.cardid);
                mpvue.navigateTo({
                    // url: '../detailforworker/main',
                    url: '../detail/main?oid=' + e.mp.currentTarget.dataset.cardid,
                })
            },
            change(e){
                this.current_scroll = e.mp.detail.current
            },
            goreport(){
                mpvue.navigateTo({
                    url: '../report/main',
                })
            },
            pswreset(){
                this.showleft = ! this.showleft
                wx.navigateTo({
                    url: '../pswreset/main'
                })
                this.showleft = ! this.showleft
            },
            changecell(){
                this.showleft = ! this.showleft
                wx.navigateTo({
                    url: '../changecell1/main'
                })
                this.showleft = ! this.showleft
            },
            logout(){
                wx.clearStorageSync()
                wx.navigateTo({
                    url: '../login/main'
                })
                this.showleft = ! this.showleft
            },
            wxGetUserInfo(code){
                const self = this;
                wx.getUserInfo({
                    withCredentials: true,
                    success(res){
                        console.log(res);
                        this.toggleLeft() // 变 化
                        wx.setStorageSync('user',res);
                        this.userSet(res)
                    },
                    fail(err){
                        console.log('自动wx.getUserInfo失败:',err);
                        // 显示主动授权的button
                        self.buttonVisible = true;
                    }
                })
            },
            bindGetUserInfo(e){
                // console.log('回调事件后触发')
                const self = this;
                if(e.mp.detail.userInfo){
                    console.log('用户按了允许授权按钮')
                    console.log(e.mp.detail.userInfo);
                    this.toggleLeft() // 变 化
                    wx.setStorageSync('user',e.mp.detail.userInfo);
                    this.userSet(e.mp.detail.userInfo)
                } else {
                    //用户按了拒绝按钮
                    console.log('用户按了拒绝按钮');
                }
            },
            userSet(res){
                this.userIcon = res.avatarUrl;
                this.usserName = res.nickName
            },
            loadmore(){
                if(! this.end){
                    this.loaddata(this.page)
                }
            },
            loaddata(page){
                var _this = this;
                var uid = wx.getStorageSync("UID");
                if(uid){
                    wx.request({
                        url: 'https://hd.xmountguan.com/railway/order.aspx?func=get_user_orders&uid=' + uid + '&orderstatus=0' + '&page=' + this.page, //仅为示例，并非真实的接口地址
                        success(res){
                            if(_this.page == 1){
                                _this.lists = []
                            }
                            var Things = res.data
                            if(Things.length < 5){
                                _this.end = true
                            } else {
                                _this.end = false
                                console.log("_this.page" + _this.page)
                                _this.page = parseInt(_this.page) + parseInt(1)
                                console.log("_this.page" + _this.page)
                            }
                            for(var i = 0; i < Things.length; i ++){
                                var statusText = Things[i].OrderStatus
                                var statuscode = ''
                                if(statusText == "待处理"){
                                    statuscode = "0"
                                } else if(statusText == "维修中"){
                                    statuscode = "1"
                                } else if(statusText == "已完成"){
                                    statuscode = "2"
                                } else if(statusText == "已中止"){
                                    statuscode = "3"
                                }
                                var json = {
                                    "listId"    : Things[i].OID,
                                    "listNumber": Things[i].SerialNo,
                                    "listTime"  : Things[i].CreateTime,
                                    "listType"  : Things[i].MaintenanceType,
                                    "listState" : statuscode,
                                    "listLoca"  : Things[i].DetailLocation
                                }
                                _this.lists.push(json)
                            }
                        }
                    })
                }
            },
            refresh(){
                var _this=this
                _this.refreshicon=true
                var wx = mpvue
                console.log('refresh');
                this.page = 1
                this.loaddata(this.page)
                wx.showToast({
                    title   : '数据刷新成功',
                    icon    : 'none',
                    duration: 1000,
                    mask    : true
                });
                setTimeout(()=>{
                    _this.refreshicon=false
                },1000)
            }
        },
        mounted(){
            console.log('mounted')
            var _this = this
            var uid = wx.getStorageSync("UID")
            if(uid){

                /*  wx.request({
					  url: 'https://hd.xmountguan.com/railway/order.aspx?func=get_user_orders&uid=' + uid + '&orderstatus=0' + '&page=' + this.page, //仅为示例，并非真实的接口地址

					  success(res) {
						  console.log(res.data)
						  var Things = res.data

						  for (var i = 0; i < Things.length; i++) {
							  console.log(Things[i])
							  var statusText = Things[i].OrderStatus
							  var statuscode = ''
							  if (statusText == "待处理") {
								  statuscode = "0"
							  } else if (statusText == "维修中") {
								  statuscode = "1"
							  } else if (statusText == "已完成") {
								  statuscode = "2"
							  } else if (statusText == "已中止") {
								  statuscode = "3"
							  }
							  var json = {
								  "listId": Things[i].OID,
								  "listNumber": Things[i].SerialNo,
								  "listTime": Things[i].CreateTime,
								  "listType": Things[i].MaintenanceType,
								  "listState": statuscode,
								  "listLoca": Things[i].DetailLocation
							  }
							  _this.lists.push(json)
						  }
					  }
				  })*/
            }
        },
        onShow(){
            this.page = 1
            console.log('show')
            this.loaddata(this.page)
            let _this = this;
            // let app = getApp()
            mpvue.getSystemInfo({
                success: function(res){
                    /* console.log(res.windowWidth);*/
                    /* console.log(res.windowHeight);*/
                    var w = res.windowWidth
                    var h = res.windowHeight
                    var calHeight = (h/w*750 - 200).toFixed(2)
                    _this.scrollData.height = calHeight + "rpx"
                    console.log(_this.scrollData.height);
                },
            })
            var login = wx.getStorageSync('login')
            console.log(login);
           /* if(! login){
                wx.redirectTo({
                    // url: '../detailforworker/main',
                    url: '../login/main',
                })
            }*/
            wx.showShareMenu();
        },
    }

</script>
<style scoped>
    .box button::after {
        border : none;
    }
    .gray {
        color : gray;
    }
    .yellow {
        color : #ff8c2e;
    }
    .green {
        color : green;
    }
    .red {
        color : #ed283c;
    }
    .miantitle {
        width      : 100%;
        display    : block;
        text-align : center;
        font-size  : 35rpx;
        position   : absolute;
        top        : 22rpx;
        left       : 0;
    }
    .demo-container {
        width           : 400rpx;
        height          : 100vh;
        background      : white;
        display         : flex;
        display         : -webkit-flex;
        justify-content : flex-start;
        align-items     : center;
        flex-flow       : column wrap;
        padding-top     : 118rpx;
    }
    .i-drawer-mask {
        background : rgba(28, 36, 56, 0.29);
    }
    .tabs {
        width     : 92rpx;
        font-size : 20rpx;
    }
    .icon {
        position : fixed;
        left     : 36rpx;
        top      : -1rpx;
        z-index  : 1;
    }
    .itabs {
        position        : fixed;
        z-index         : 2;
        width           : 648rpx;
        box-sizing      : border-box;
        top             : 0;
        left            : 102rpx;
        justify-content : space-around;
        display         : block;
    }
    .usericon {
        width                 : 112rpx;
        height                : 112rpx;
        /*border:2rpx solid #08a7e2; */
        border-radius         : 100%;
        -webkit-border-radius : 100%;
    }
    .username {
        display    : block;
        margin-top : 20rpx;
        font-size  : 28rpx;
        color      : #595959;
    }
    .cz {
        height      : 50rpx;
        line-height : 50rpx;
        width       : 80%;
        margin-top  : 40rpx;
        font-size   : 28rpx;
        color       : #595959;
    }
    .chongzhiCell {
        margin-top : 20rpx;
    }
    .card {
        display               : block;
        width                 : 730rpx;
        height                : 320rpx;
        /*background: #595959;*/
        margin                : 0 auto;
        margin-bottom         : 10rpx;
        border                : 1rpx solid rgba(128, 128, 128, 0.53);
        position              : relative;
        border-radius         : 10rpx;
        -webkit-border-radius : 10rpx;
        background            : white;
        position              : relative;
    }
    .tabscontent {
        display         : flex;
        justify-content : center;
        flex-flow       : column wrapper;
    }
    .danhao {
        position  : absolute;
        left      : 31rpx;
        top       : 28rpx;
        font-size : 28rpx;
    }
    .state {
        position  : absolute;
        right     : 31rpx;
        top       : 28rpx;
        font-size : 28rpx;
    }
    .listtime,
    .listtype,
    .listlocation {
        position    : absolute;
        left        : 26rpx;
        top         : 86rpx;
        display     : block;
        font-size   : 32rpx;
        line-height : 40rpx;
        height      : 56rpx;
        overflow    : hidden;
    }
    .listtype {
        top : 162rpx;
    }
    .listlocation {
        top : 232rpx;
    }
    .listicon {
        font-size : 29px;
        color     : #ACACAC;
        position  : relative;
        top       : 4rpx;
    }
    .tdetail {
        margin-left : 10rpx;
        font-size   : 28rpx;
    }
    .swiper {
        width       : 100vw;
        display     : block;
        overflow    : hidden;
        height      : 100vh;
        position    : fixed;
        left        : 0;
        top         : 0;
        padding-top : 85rpx;
        box-sizing  : border-box;
    }
    .swiperitem {
        width          : 100%;
        height         : 100%;
        overflow-x     : hidden;
        overflow-y     : hidden;
        display        : block;
        position       : relative;
        padding-bottom : 30rpx;
    }
    .goreport {
        width    : 100%;
        position : fixed;
        left     : 0;
        bottom   : 0;
    }
    .spin {
        animation         : spinspin 2s infinite linear;
        -webkit-animation : spinspin 2s infinite linear;
    }
    @keyframes spinspin {
        from {
            opacity           : 0;
            transform         : rotate(0);
            -webkit-transform : rotate(0);
        }
        from {
            opacity           : 1;
            transform         : rotate(360deg);
            -webkit-transform : rotate(360deg);
        }

    }
    @-webkit-keyframes spinspin {
        from {
            opacity           : 0;
            transform         : rotate(0);
            -webkit-transform : rotate(0);
        }
        from {
            opacity           : 1;
            transform         : rotate(360deg);
            -webkit-transform : rotate(360deg);
        }

    }
</style>
