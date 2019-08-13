<template>
    <div class=" box">

        <button open-type="getUserInfo" @getuserinfo="bindGetUserInfo" @click="getUserInfoClick"
                style="width: 100rpx;background: transparent;height: 100rpx;position: fixed;z-index: 2;border: none">
            <i-icon type="mine" size="26" color="#ACACAC" class="icon"/>
        </button>
        <i-tabs class='itabs' :current="current_scroll" @change="handleChangeScroll">
            <i-tab :key="index" class="tabs" v-for="(item,index) in navtabs" :title="item"></i-tab>
        </i-tabs>
        <swiper class="swiper" duration="300" :current="current_scroll" @change="change">
            <swiper-item class="swiperitem">
                <!--                <span v-if="refreshicon" class="freshicon"> 刷新中...<i-icon size='14' type="refresh"/></span>-->
                <scroll-view scroll-y="true" :style="computedClassObject" :upper-threshold="'350'"
                             @scrolltoupper="refresh" @scrolltolower="loadmore">

                    <view v-if="lists.length>0">
                        <!---->
                        <div class="card" :data-cardid="item.listId" :data-cardindex="index"
                             ref="dataNum" @click="cardClick($event,item.listState)" v-for="(item,index) in  lists"
                             :key="index">
                            <span class="danhao">报修单号:{{item.listNumber}}</span>
                            <span
                                :class="[{  gray  : item.listState==='0' },{  yellow  : item.listState==='1' },{  green  : item.listState==='2' },{  red  : item.listState==='3' }, 'state']">{{item.listState==0?"待处理":(item.listState==1?"维修中":(item.listState==2?"已完成":"已中止"))}}</span>
                            <div class="listtime">
                                <i-icon type="time" size="29" color="#ACACAC" class="listicon"/>
                                <span class="tdetail">{{item.listTime}}</span>
                            </div>
                            <div class="listtype">
                                <i-icon type="setup_fill" size="29" color="#ACACAC" class="listicon"/>
                                <span class="tdetail">{{item.listType}}</span>
                            </div>
                            <div class="listlocation">
                                <i-icon type="coordinates_fill" size="29" color="#ACACAC" class="listicon"/>
                                <view class="tdetail">{{item.listLoca}}</view>
                            </div>
                        </div>
                    </view>
                    <view v-else class="nodata">暂无数据</view>
                </scroll-view>
            </swiper-item>
            <swiper-item class="swiperitem">
                <!--                <span v-if="refreshicon" class="freshicon"> 刷新中...<i-icon size='14' type="refresh"/></span>-->
                <scroll-view :scroll-y="scrolly" :style="computedClassObject"
                             :upper-threshold="'350'"
                             @scrolltoupper="refresh" @scrolltolower="loadmore1">
                    <view v-if="lists1.length>0">


                        <div class="card" :data-cardid="item.listId" ref="dataNum" :data-cardindex="index"
                             @click="cardClick($event,item.listState)" v-for="(item,index) in  lists1" :key="index"
                             v-if="item.listState==0">
                            <span class="danhao">报修单号:{{item.listNumber}}</span>
                            <span
                                :class="[{  gray  : item.listState==='0' },{  yellow  : item.listState==='1' },{  green  : item.listState==='2' },{  red  : item.listState==='3' }, 'state']">{{item.listState==0?"待处理":(item.listState==1?"维修中":(item.listState==2?"已完成":"已中止"))}}</span>
                            <div class="listtime">
                                <i-icon type="time" size="29" color="#ACACAC" class="listicon"/>
                                <span class="tdetail">{{item.listTime}}</span>
                            </div>
                            <div class="listtype">
                                <i-icon type="setup_fill" size="29" color="#ACACAC" class="listicon"/>
                                <span class="tdetail">{{item.listType}}</span>
                            </div>
                            <div class="listlocation">
                                <i-icon type="coordinates_fill" size="29" color="#ACACAC" class="listicon"/>
                                <view class="tdetail">{{item.listLoca}}</view>
                            </div>
                        </div>
                    </view>
                    <view v-else class="nodata">暂无数据</view>
                </scroll-view>

            </swiper-item>
            <swiper-item class="swiperitem">
                <!--                <span v-if="refreshicon" class="freshicon"> 刷新中...<i-icon size='14' type="refresh"/></span>-->
                <scroll-view :scroll-y="scrolly" :style="computedClassObject"
                             :upper-threshold="'350'"
                             @scrolltoupper="refresh" @scrolltolower="loadmore2">
                    <view v-if="lists2.length>0">

                        <div class="card" :data-cardid="item.listId" ref="dataNum" :data-cardindex="index"
                             @click="cardClick($event,item.listState)" v-for="(item,index) in  lists2" :key="index"
                             v-if="item.listState==1">
                            <span class="danhao">报修单号:{{item.listNumber}}</span>
                            <span
                                :class="[{  gray  : item.listState==='0' },{  yellow  : item.listState==='1' },{  green  : item.listState==='2' },{  red  : item.listState==='3' }, 'state']">{{item.listState==0?"待处理":(item.listState==1?"维修中":(item.listState==2?"已完成":"已中止"))}}</span>
                            <div class="listtime">
                                <i-icon type="time" size="29" color="#ACACAC" class="listicon"/>
                                <span class="tdetail">{{item.listTime}}</span>
                            </div>
                            <div class="listtype">
                                <i-icon type="setup_fill" size="29" color="#ACACAC" class="listicon"/>
                                <span class="tdetail">{{item.listType}}</span>
                            </div>
                            <div class="listlocation">
                                <i-icon type="coordinates_fill" size="29" color="#ACACAC" class="listicon"/>
                                <view class="tdetail">{{item.listLoca}}</view>
                            </div>
                        </div>
                    </view>
                    <view v-else class="nodata">暂无数据</view>
                </scroll-view>
            </swiper-item>
            <swiper-item class="swiperitem">
                <!--                <span v-if="refreshicon" class="freshicon"> 刷新中...<i-icon size='14' type="refresh"/></span>-->
                <scroll-view :scroll-y="scrolly" :style="computedClassObject"
                             :upper-threshold="'350'"
                             @scrolltoupper="refresh" @scrolltolower="loadmore3">
                    <view v-if="lists3.length>0">


                        <div class="card" :data-cardid="item.listId" ref="dataNum" :data-cardindex="index"
                             @click="cardClick($event,item.listState)" v-for="(item,index) in  lists3" :key="index"
                             v-if="item.listState==2">
                            <span class="danhao">报修单号:{{item.listNumber}}</span>
                            <span
                                :class="[{  gray  : item.listState==='0' },{  yellow  : item.listState==='1' },{  green  : item.listState==='2' },{  red  : item.listState==='3' }, 'state']">{{item.listState==0?"待处理":(item.listState==1?"维修中":(item.listState==2?"已完成":"已中止"))}}</span>
                            <div class="listtime">
                                <i-icon type="time" size="29" color="#ACACAC" class="listicon"/>
                                <span class="tdetail">{{item.listTime}}</span>
                            </div>
                            <div class="listtype">
                                <i-icon type="setup_fill" size="29" color="#ACACAC" class="listicon"/>
                                <span class="tdetail">{{item.listType}}</span>
                            </div>
                            <div class="listlocation">
                                <i-icon type="coordinates_fill" size="29" color="#ACACAC" class="listicon"/>
                                <view class="tdetail">{{item.listLoca}}</view>
                            </div>
                        </div>
                    </view>
                    <view v-else class="nodata">暂无数据</view>
                </scroll-view>
            </swiper-item>
            <swiper-item class="swiperitem">
                <!--                <span v-if="refreshicon" class="freshicon"> 刷新中...<i-icon size='14' type="refresh"/></span>-->
                <scroll-view :scroll-y="scrolly" :style="computedClassObject"
                             :upper-threshold="'350'"
                             @scrolltoupper="refresh" @scrolltolower="loadmore4">
                    <view v-if="lists4.length>0">

                        <div class="card" :data-cardid="item.listId" ref="dataNum" :data-cardindex="index"
                             @click="cardClick($event,item.listState)" v-for="(item,index) in  lists4" :key="index"
                             v-if="item.listState==3">
                            <span class="danhao">报修单号:{{item.listNumber}}</span>
                            <span
                                :class="[{  gray  : item.listState==='0' },{  yellow  : item.listState==='1' },{  green  : item.listState==='2' },{  red  : item.listState==='3' }, 'state']">{{item.listState==0?"待处理":(item.listState==1?"维修中":(item.listState==2?"已完成":"已中止"))}}</span>
                            <div class="listtime">
                                <i-icon type="time" size="29" color="#ACACAC" class="listicon"/>
                                <span class="tdetail">{{item.listTime}}</span>
                            </div>
                            <div class="listtype">
                                <i-icon type="setup_fill" size="29" color="#ACACAC" class="listicon"/>
                                <span class="tdetail">{{item.listType}}</span>
                            </div>
                            <div class="listlocation">
                                <i-icon type="coordinates_fill" size="29" color="#ACACAC" class="listicon"/>
                                <view class="tdetail">{{item.listLoca}}</view>
                            </div>
                        </div>
                    </view>
                    <view v-else class="nodata">暂无数据</view>
                </scroll-view>
            </swiper-item>
        </swiper>
        <i-button @click="goreport" class="goreport" type="primary">一键报修</i-button>
        <i-drawer mode="left" @click="toggleLeft" :visible="showleft" class="i-drawer-mask">
            <div class="demo-container" @click.stop>
                <img v-if="usershow" :src="userIcon" class="usericon" @click="changeIcon" alt=""/>
                <span v-if="usershow" class="username">{{usserName}}</span>
                <view v-if="login" style="width: 80%">
                    <view class="cz chongzhiPsw" @click="pswreset">
                        <i-icon type="lock_fill" size="21" color="#ACACAC"/>
                        重置密码
                    </view>
                    <view class="cz chongzhiCell" @click="changecell">
                        <i-icon type="mobilephone_fill" size="21" color="#ACACAC"/>
                        更换绑定手机号
                    </view>
                    <view class="cz chongzhiCell" @click="logout">
                        <i-icon type="offline_fill" size="21" color="#ACACAC"/>
                        退出
                    </view>
                </view>
                <view v-else style="width: 80%">
                    <view class="cz chongzhiCell" @click="logout">
                        <i-icon type="group" size="21" color="#ACACAC"/>
                        登录
                    </view>
                </view>

            </div>
        </i-drawer>
    </div>
</template>
<script>
    var timeout0 = ''
    var timeout1 = ''
    var timeout2 = ''
    var timeout3 = ''
    var timeout4 = ''
    export default {
        data() {
            return {
                refreshicon: false,
                navtabs: ["全部", "待处理", "维修中", "已完成", "已中止"],
                Role: 2,
                showleft: false,
                user: "",
                usershow: false,
                current_scroll: '0',
                userIcon: '/static/images/Avator.png',
                usserName: "一万年朝夕",
                scrolly: true,
                scrollData: {
                    height: '1100rpx',
                    "-webkit-overflow-scrolling": "touch",
                },
                lists: [],
                lists1: [],
                lists2: [],
                lists3: [],
                lists4: [],
                page: '1',
                page1: '1',
                page2: '1',
                page3: '1',
                page4: '1',
                end: false,
                end1: false,
                end2: false,
                end3: false,
                end4: false,
                login: ""
            }
        },
        components: {},
        computed: {
            classObject: function () {
                return {
                    active: this.isActive,
                    sort: this.isSort,
                }
            },
            computedClassObject() {
                return this.showJson({
                    height: this.scrollData.height,
                })
            }
        },
        methods: {
            showJson(style) {
                let s = []
                for (let i in style) {
                    s.push(i + ':' + style[i]);
                }
                s = s.join(';')
                return s
            },
            toggleLeft(e) {
                this.showleft = !this.showleft
            },
            refresh() {
                // var _this = this
                // _this.refreshicon = true
                // var wx = mpvue
                // console.log('refresh');
                // setTimeout(() => {
                //     _this.refreshicon = false
                // }, 1000)
                // var uid = wx.getStorageSync("UID");
                // if (uid) {
                //     wx.request({
                //         url: 'https://hd.xmountguan.com/railway/order.aspx?func=get_user_orders&uid=' + uid + '&orderstatus=0' + '&page=1&pagesize=' + this.page * 5, //仅为示例，并非真实的接口地址
                //         success(res) {
                //             console.log('refreshing data');
                //             // var Things =
                //             //
                //             // console.log(res.data)
                //             var databack = res.data
                //             var statuscode = ''
                //             if (databack.length < 5) {
                //                 _this.end = true
                //             } else {
                //                 _this.end = false
                //                 _this.page = parseInt(_this.page) + 1
                //             }
                //             _this.lists = []
                //             for (var i = 0; i < databack.length; i++) {
                //                 // for(let i = 0 ; i < 2; i++){
                //                 // console.log(databack[i]);
                //                 (function () {
                //                     var j = i;
                //                     setTimeout(function timer() {
                //                         if (databack[j].OrderStatus == "待处理") {
                //                             statuscode = "0"
                //                         } else if (databack[j].OrderStatus == "维修中") {
                //                             statuscode = "1"
                //                         } else if (databack[j].OrderStatus == "已完成") {
                //                             statuscode = "2"
                //                         } else if (databack[j].OrderStatus == "已中止") {
                //                             statuscode = "3"
                //                         }
                //                         var json = {
                //                             "listId": databack[j].OID,
                //                             "listNumber": databack[j].SerialNo,
                //                             "listTime": databack[j].CreateTime,
                //                             "listType": databack[j].MaintenanceType,
                //                             "listState": statuscode,
                //                             "listLoca": databack[j].DetailLocation
                //                         }
                //                         _this.lists.push(json)
                //                     }, i * 100); //这一行将i*1000改为j*1000也行，并不影响
                //                 })();
                //             }
                //         }
                //     })
                // }
            },
            handleChangeScroll(e) {
                this.current_scroll = e.target.key;
                // console.log('slide' + this.current_scroll)
                // var _this = this;
                // var uid = wx.getStorageSync("UID");
                // if (uid) {
                //     _this.lists = []
                //     _this.lists1 = []
                //     _this.lists2 = []
                //     _this.lists3 = []
                //     _this.lists4 = []
                // }
                // _this.loaddatas()
            },
            loaddatas() {
                var _this = this
                var uid = wx.getStorageSync("UID")
                if (uid) {
                    if (_this.current_scroll == 0) {
                        console.log("00000000000000")
                        wx.request({
                            url: 'https://hd.xmountguan.com/railway/order.aspx?func=get_user_orders&uid=' + uid + '&orderstatus=0' + '&page=1&pagesize=5', //仅为示例，并非真实的接口地址
                            success(res) {
                                console.log('refreshing data');
                                // var Things =
                                //
                                console.log(res.data)
                                var databack = res.data
                                var statuscode = ''
                                if (databack.length < 5) {
                                    _this.end = true
                                } else {
                                    _this.end = false
                                    _this.page = parseInt(_this.page) + 1
                                }
                                _this.lists = []
                                if (timeout0)
                                    clearTimeout(timeout0)
                                for (var i = 0; i < databack.length; i++) {
                                    // for(let i = 0 ; i < 2; i++){
                                    // console.log(databack[i]);
                                    (function () {
                                        var j = i;

                                        timeout0 = setTimeout(function timer() {
                                            if (databack[j].OrderStatus == "待处理") {
                                                statuscode = "0"
                                            } else if (databack[j].OrderStatus == "维修中") {
                                                statuscode = "1"
                                            } else if (databack[j].OrderStatus == "已完成") {
                                                statuscode = "2"
                                            } else if (databack[j].OrderStatus == "已中止") {
                                                statuscode = "3"
                                            }
                                            var json = {
                                                "listId": databack[j].OID,
                                                "listNumber": databack[j].SerialNo,
                                                "listTime": databack[j].CreateTime,
                                                "listType": databack[j].MaintenanceType,
                                                "listState": statuscode,
                                                "listLoca": databack[j].DetailLocation
                                            }
                                            _this.lists.push(json)
                                        }, i * 100); //这一行将i*1000改为j*1000也行，并不影响
                                    })();
                                }
                            }
                        })
                    } else {

                            if (_this.current_scroll == 1) {
                                console.log("111111111111111")
                                wx.request({
                                    url: 'https://hd.xmountguan.com/railway/order.aspx?func=get_user_orders&uid=' + uid + '&orderstatus=1' + '&page=1&pagesize=' + this.page1 * 5, //仅为示例，并非真实的接口地址
                                    success(res) {
                                        console.log('refreshing data');
                                        // var Things =
                                        //
                                        console.log(res.data)
                                        var databack = res.data
                                        var statuscode = ''
                                        if (databack.length < 5) {
                                            _this.end1 = true
                                        } else {
                                            _this.end1 = false
                                            _this.page1 = parseInt(_this.page1) + 1
                                        }
                                        _this.lists1 = []
                                        if (timeout0)
                                            clearTimeout(timeout0)
                                        for (var i = 0; i < databack.length; i++) {
                                            // for(let i = 0 ; i < 2; i++){
                                            // console.log(databack[i]);
                                            (function () {
                                                var j = i;

                                                timeout0 = setTimeout(function timer() {
                                                    if (databack[j].OrderStatus == "待处理") {
                                                        statuscode = "0"
                                                    } else if (databack[j].OrderStatus == "维修中") {
                                                        statuscode = "1"
                                                    } else if (databack[j].OrderStatus == "已完成") {
                                                        statuscode = "2"
                                                    } else if (databack[j].OrderStatus == "已中止") {
                                                        statuscode = "3"
                                                    }
                                                    var json = {
                                                        "listId": databack[j].OID,
                                                        "listNumber": databack[j].SerialNo,
                                                        "listTime": databack[j].CreateTime,
                                                        "listType": databack[j].MaintenanceType,
                                                        "listState": statuscode,
                                                        "listLoca": databack[j].DetailLocation
                                                    }
                                                    _this.lists1.push(json)
                                                }, i * 100); //这一行将i*1000改为j*1000也行，并不影响
                                            })();
                                        }
                                    }
                                })
                            } else if (_this.current_scroll == 2) {
                                console.log("222222222222222")
                                wx.request({
                                    url: 'https://hd.xmountguan.com/railway/order.aspx?func=get_user_orders&uid=' + uid + '&orderstatus=2' + '&page=1&pagesize=5', //仅为示例，并非真实的接口地址
                                    success(res) {
                                        console.log('refreshing data');
                                        // var Things =
                                        //
                                        console.log(res.data)
                                        var databack = res.data
                                        var statuscode = ''
                                        if (databack.length < 5) {
                                            _this.end2 = true
                                        } else {
                                            _this.end2 = false
                                            _this.page2 = parseInt(_this.page2) + 1
                                        }
                                        _this.lists2 = []
                                        if (timeout0)
                                            clearTimeout(timeout0)
                                        for (var i = 0; i < databack.length; i++) {
                                            // for(let i = 0 ; i < 2; i++){
                                            // console.log(databack[i]);
                                            (function () {
                                                var j = i;

                                                timeout0 = setTimeout(function timer() {
                                                    if (databack[j].OrderStatus == "待处理") {
                                                        statuscode = "0"
                                                    } else if (databack[j].OrderStatus == "维修中") {
                                                        statuscode = "1"
                                                    } else if (databack[j].OrderStatus == "已完成") {
                                                        statuscode = "2"
                                                    } else if (databack[j].OrderStatus == "已中止") {
                                                        statuscode = "3"
                                                    }
                                                    var json = {
                                                        "listId": databack[j].OID,
                                                        "listNumber": databack[j].SerialNo,
                                                        "listTime": databack[j].CreateTime,
                                                        "listType": databack[j].MaintenanceType,
                                                        "listState": statuscode,
                                                        "listLoca": databack[j].DetailLocation
                                                    }
                                                    _this.lists2.push(json)
                                                }, i * 100); //这一行将i*1000改为j*1000也行，并不影响
                                            })();
                                        }
                                    }
                                })
                            } else if (_this.current_scroll == 3) {
                                console.log("333333333333333333")
                                wx.request({
                                    url: 'https://hd.xmountguan.com/railway/order.aspx?func=get_user_orders&uid=' + uid + '&orderstatus=3' + '&page=1&pagesize=' + this.page3 * 5, //仅为示例，并非真实的接口地址
                                    success(res) {
                                        console.log('refreshing data');
                                        // var Things =
                                        //
                                        console.log(res.data)
                                        var databack = res.data
                                        var statuscode = ''
                                        if (databack.length < 5) {
                                            _this.end = true
                                        } else {
                                            _this.end3 = false
                                            _this.page3 = parseInt(_this.page2) + 1
                                        }
                                        _this.lists3 = []
                                        if (timeout0)
                                            clearTimeout(timeout0)
                                        for (var i = 0; i < databack.length; i++) {
                                            // for(let i = 0 ; i < 2; i++){
                                            // console.log(databack[i]);
                                            (function () {
                                                var j = i;

                                                timeout0 = setTimeout(function timer() {
                                                    if (databack[j].OrderStatus == "待处理") {
                                                        statuscode = "0"
                                                    } else if (databack[j].OrderStatus == "维修中") {
                                                        statuscode = "1"
                                                    } else if (databack[j].OrderStatus == "已完成") {
                                                        statuscode = "2"
                                                    } else if (databack[j].OrderStatus == "已中止") {
                                                        statuscode = "3"
                                                    }
                                                    var json = {
                                                        "listId": databack[j].OID,
                                                        "listNumber": databack[j].SerialNo,
                                                        "listTime": databack[j].CreateTime,
                                                        "listType": databack[j].MaintenanceType,
                                                        "listState": statuscode,
                                                        "listLoca": databack[j].DetailLocation
                                                    }
                                                    _this.lists3.push(json)
                                                }, i * 100); //这一行将i*1000改为j*1000也行，并不影响
                                            })();
                                        }
                                    }
                                })
                            } else if (_this.current_scroll == 4) {
                                console.log("444444444444444")
                                wx.request({
                                    url: 'https://hd.xmountguan.com/railway/order.aspx?func=get_user_orders&uid=' + uid + '&orderstatus=4' + '&page=1&pagesize=5', //仅为示例，并非真实的接口地址
                                    success(res) {
                                        console.log('refreshing data');
                                        // var Things =
                                        //
                                        console.log(res.data)
                                        var databack = res.data
                                        var statuscode = ''
                                        if (databack.length < 5) {
                                            _this.end4 = true
                                        } else {
                                            _this.end4 = false
                                            _this.page4 = parseInt(_this.page4) + 1
                                        }
                                        _this.lists4 = []
                                        if (timeout0)
                                            clearTimeout(timeout0)
                                        for (var i = 0; i < databack.length; i++) {
                                            // for(let i = 0 ; i < 2; i++){
                                            // console.log(databack[i]);
                                            (function () {
                                                var j = i;

                                                timeout0 = setTimeout(function timer() {
                                                    if (databack[j].OrderStatus == "待处理") {
                                                        statuscode = "0"
                                                    } else if (databack[j].OrderStatus == "维修中") {
                                                        statuscode = "1"
                                                    } else if (databack[j].OrderStatus == "已完成") {
                                                        statuscode = "2"
                                                    } else if (databack[j].OrderStatus == "已中止") {
                                                        statuscode = "3"
                                                    }
                                                    var json = {
                                                        "listId": databack[j].OID,
                                                        "listNumber": databack[j].SerialNo,
                                                        "listTime": databack[j].CreateTime,
                                                        "listType": databack[j].MaintenanceType,
                                                        "listState": statuscode,
                                                        "listLoca": databack[j].DetailLocation
                                                    }
                                                    _this.lists4.push(json)
                                                }, i * 100); //这一行将i*1000改为j*1000也行，并不影响
                                            })();
                                        }
                                    }
                                })
                            }

                    }

                    /*    if (_this.Role == 1) {
                            if (_this.current_scroll + 1 == 1) {
                                console.log("111111111111111")
                                wx.request({
                                    url: 'https://hd.xmountguan.com/railway/order.aspx?func=get_user_orders&uid=' + uid + '&orderstatus=1' + '&page=1&pagesize=' + this.page1 * 5, //仅为示例，并非真实的接口地址
                                    success(res) {
                                        console.log('refreshing data');
                                        // var Things =
                                        //
                                        console.log(res.data)
                                        var databack = res.data
                                        var statuscode = ''
                                        if (databack.length < 5) {
                                            _this.end = true
                                        } else {
                                            _this.end = false
                                            _this.page1 = parseInt(_this.page1) + 1
                                        }
                                        _this.lists1 = []
                                        if (timeout0)
                                            clearTimeout(timeout0)
                                        for (var i = 0; i < databack.length; i++) {
                                            // for(let i = 0 ; i < 2; i++){
                                            // console.log(databack[i]);
                                            (function () {
                                                var j = i;

                                                timeout0 = setTimeout(function timer() {
                                                    if (databack[j].OrderStatus == "待处理") {
                                                        statuscode = "0"
                                                    } else if (databack[j].OrderStatus == "维修中") {
                                                        statuscode = "1"
                                                    } else if (databack[j].OrderStatus == "已完成") {
                                                        statuscode = "2"
                                                    } else if (databack[j].OrderStatus == "已中止") {
                                                        statuscode = "3"
                                                    }
                                                    var json = {
                                                        "listId": databack[j].OID,
                                                        "listNumber": databack[j].SerialNo,
                                                        "listTime": databack[j].CreateTime,
                                                        "listType": databack[j].MaintenanceType,
                                                        "listState": statuscode,
                                                        "listLoca": databack[j].DetailLocation
                                                    }
                                                    _this.lists1.push(json)
                                                }, i * 100); //这一行将i*1000改为j*1000也行，并不影响
                                            })();
                                        }
                                    }
                                })
                            } else if (_this.current_scroll + 1 == 2) {
                                console.log("222222222222222")
                                wx.request({
                                    url: 'https://hd.xmountguan.com/railway/order.aspx?func=get_user_orders&uid=' + uid + '&orderstatus=2' + '&page=1&pagesize=5', //仅为示例，并非真实的接口地址
                                    success(res) {
                                        console.log('refreshing data');
                                        // var Things =
                                        //
                                        console.log(res.data)
                                        var databack = res.data
                                        var statuscode = ''
                                        if (databack.length < 5) {
                                            _this.end = true
                                        } else {
                                            _this.end = false
                                            _this.page2 = parseInt(_this.page2) + 1
                                        }
                                        _this.lists2 = []
                                        if (timeout0)
                                            clearTimeout(timeout0)
                                        for (var i = 0; i < databack.length; i++) {
                                            // for(let i = 0 ; i < 2; i++){
                                            // console.log(databack[i]);
                                            (function () {
                                                var j = i;

                                                timeout0 = setTimeout(function timer() {
                                                    if (databack[j].OrderStatus == "待处理") {
                                                        statuscode = "0"
                                                    } else if (databack[j].OrderStatus == "维修中") {
                                                        statuscode = "1"
                                                    } else if (databack[j].OrderStatus == "已完成") {
                                                        statuscode = "2"
                                                    } else if (databack[j].OrderStatus == "已中止") {
                                                        statuscode = "3"
                                                    }
                                                    var json = {
                                                        "listId": databack[j].OID,
                                                        "listNumber": databack[j].SerialNo,
                                                        "listTime": databack[j].CreateTime,
                                                        "listType": databack[j].MaintenanceType,
                                                        "listState": statuscode,
                                                        "listLoca": databack[j].DetailLocation
                                                    }
                                                    _this.lists2.push(json)
                                                }, i * 100); //这一行将i*1000改为j*1000也行，并不影响
                                            })();
                                        }
                                    }
                                })
                            } else if (_this.current_scroll + 1 == 3) {
                                console.log("333333333333333333")
                                wx.request({
                                    url: 'https://hd.xmountguan.com/railway/order.aspx?func=get_user_orders&uid=' + uid + '&orderstatus=3' + '&page=1&pagesize=' + this.page3 * 5, //仅为示例，并非真实的接口地址
                                    success(res) {
                                        console.log('refreshing data');
                                        // var Things =
                                        //
                                        console.log(res.data)
                                        var databack = res.data
                                        var statuscode = ''
                                        if (databack.length < 5) {
                                            _this.end = true
                                        } else {
                                            _this.end = false
                                            _this.page3 = parseInt(_this.page2) + 1
                                        }
                                        _this.lists3 = []
                                        if (timeout0)
                                            clearTimeout(timeout0)
                                        for (var i = 0; i < databack.length; i++) {
                                            // for(let i = 0 ; i < 2; i++){
                                            // console.log(databack[i]);
                                            (function () {
                                                var j = i;

                                                timeout0 = setTimeout(function timer() {
                                                    if (databack[j].OrderStatus == "待处理") {
                                                        statuscode = "0"
                                                    } else if (databack[j].OrderStatus == "维修中") {
                                                        statuscode = "1"
                                                    } else if (databack[j].OrderStatus == "已完成") {
                                                        statuscode = "2"
                                                    } else if (databack[j].OrderStatus == "已中止") {
                                                        statuscode = "3"
                                                    }
                                                    var json = {
                                                        "listId": databack[j].OID,
                                                        "listNumber": databack[j].SerialNo,
                                                        "listTime": databack[j].CreateTime,
                                                        "listType": databack[j].MaintenanceType,
                                                        "listState": statuscode,
                                                        "listLoca": databack[j].DetailLocation
                                                    }
                                                    _this.lists3.push(json)
                                                }, i * 100); //这一行将i*1000改为j*1000也行，并不影响
                                            })();
                                        }
                                    }
                                })
                            } else if (_this.current_scroll + 1 == 4) {
                                console.log("444444444444444")
                                wx.request({
                                    url: 'https://hd.xmountguan.com/railway/order.aspx?func=get_user_orders&uid=' + uid + '&orderstatus=4' + '&page=1&pagesize=5', //仅为示例，并非真实的接口地址
                                    success(res) {
                                        console.log('refreshing data');
                                        // var Things =
                                        //
                                        console.log(res.data)
                                        var databack = res.data
                                        var statuscode = ''
                                        if (databack.length < 5) {
                                            _this.end = true
                                        } else {
                                            _this.end = false
                                            _this.page4 = parseInt(_this.page4) + 1
                                        }
                                        _this.lists4 = []
                                        if (timeout0)
                                            clearTimeout(timeout0)
                                        for (var i = 0; i < databack.length; i++) {
                                            // for(let i = 0 ; i < 2; i++){
                                            // console.log(databack[i]);
                                            (function () {
                                                var j = i;

                                                timeout0 = setTimeout(function timer() {
                                                    if (databack[j].OrderStatus == "待处理") {
                                                        statuscode = "0"
                                                    } else if (databack[j].OrderStatus == "维修中") {
                                                        statuscode = "1"
                                                    } else if (databack[j].OrderStatus == "已完成") {
                                                        statuscode = "2"
                                                    } else if (databack[j].OrderStatus == "已中止") {
                                                        statuscode = "3"
                                                    }
                                                    var json = {
                                                        "listId": databack[j].OID,
                                                        "listNumber": databack[j].SerialNo,
                                                        "listTime": databack[j].CreateTime,
                                                        "listType": databack[j].MaintenanceType,
                                                        "listState": statuscode,
                                                        "listLoca": databack[j].DetailLocation
                                                    }
                                                    _this.lists4.push(json)
                                                }, i * 100); //这一行将i*1000改为j*1000也行，并不影响
                                            })();
                                        }
                                    }
                                })
                            }
                        } else {*/

                }
            },
            changeIcon(e) {
                console.log(e);
            },
            handleState(state) {
                // 待处理 0 灰色
                // 维修中 1 黄色
                // 已完成 2  绿色
                // 已中止 3  红色
                switch (state) {
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
            cardClick(e, clcikstate) {
                // console.log(e.currentTarget.dataset.cardid);
                console.log(e);
                console.log("cardid is " + e.mp.currentTarget.dataset.cardid);
                console.log("cardidindex is " + e.mp.currentTarget.dataset.cardindex);
                wx.setStorageSync('cardinex', e.mp.currentTarget.dataset.cardindex);
                mpvue.navigateTo({
                    // url: '../detailforworker/main',
                    url: '../detail/main?oid=' + e.mp.currentTarget.dataset.cardid,
                })
            },
            change(e) {
                this.current_scroll = e.mp.detail.current
                var _this = this;
                var uid = wx.getStorageSync("UID");
                if (uid) {
                    _this.lists = []
                    _this.lists1 = []
                    _this.lists2 = []
                    _this.lists3 = []
                    _this.lists4 = []
                }
                _this.loaddatas()
            },
            goreport() {
                mpvue.navigateTo({
                    url: '../report/main?fromreport=' + false,
                })
            },
            pswreset() {
                this.showleft = false
                wx.navigateTo({
                    url: '../pswreset/main'
                })
            },
            changecell() {
                this.showleft = false
                wx.navigateTo({
                    url: '../changecell1/main'
                })
            },
            logout() {
                this.showleft = false
                wx.setStorageSync("UID", '')
                wx.redirectTo({
                    url: '../login/main'
                })
            },
            wxGetUserInfo(code) {
                const self = this;
                wx.getUserInfo({
                    withCredentials: true,
                    success(res) {
                        console.log(res);
                        this.toggleLeft() // 变 化
                        wx.setStorageSync('user', res);
                        this.userSet(res)
                    },
                    fail(err) {
                        console.log('自动wx.getUserInfo失败:', err);
                        // 显示主动授权的button
                        self.buttonVisible = true;
                    }
                })
            },
            bindGetUserInfo(e) {
                // console.log('回调事件后触发')
                const self = this;
                if (e.mp.detail.userInfo) {
                    console.log('用户按了允许授权按钮')
                    console.log(e.mp.detail.userInfo);
                    this.toggleLeft() // 变 化
                    wx.setStorageSync('user', e.mp.detail.userInfo);
                    this.userSet(e.mp.detail.userInfo)
                    this.usershow = true
                } else {
                    this.toggleLeft() // 变 化
                    //用户按了拒绝按钮
                    console.log('用户按了拒绝按钮');
                }
            },
            userSet(res) {
                this.userIcon = res.avatarUrl;
                this.usserName = res.nickName
            },
            loadmore() {
                console.log("loadmore")
                if (!this.end) {
                    this.loaddata(this.page)
                }
            },
            loadmore1() {       console.log("loadmore1")
                if (!this.end1) {
                    this.loaddata1(this.page1)
                }
            },
            loadmore2() {       console.log("loadmore2")
                if (!this.end2) {
                    this.loaddata2(this.page2)
                }
            },
            loadmore3() {       console.log("loadmore3")
                if (!this.end3) {
                    this.loaddata3(this.page3)
                }
            },
            loadmore4() {       console.log("loadmore4")
                if (!this.end4) {
                    this.loaddata4(this.page4)
                }
            },
            loaddata(page) {
                var _this = this;
                var uid = wx.getStorageSync("UID");
                if (uid) {
                    wx.request({
                        url: 'https://hd.xmountguan.com/railway/order.aspx?func=get_user_orders&uid=' + uid + '&orderstatus=0' + '&page=' + this.page + '&pagesize=5', //仅为示例，并非真实的接口地址
                        success(res) {

                            // var Things =
                            //
                            console.log(res.data)
                            var databack = res.data
                            var statuscode = ''
                            if (databack.length < 5) {
                                _this.end = true
                            } else {
                                _this.end = false
                                _this.page = parseInt(_this.page) + 1
                            }
                            if (timeout0)
                                clearTimeout(timeout0)
                            for (var i = 0; i < databack.length; i++) {
                                // for(let i = 0 ; i < 2; i++){
                                // console.log(databack[i]);
                                (function () {
                                    var j = i;

                                    timeout0 = setTimeout(function timer() {
                                        if (databack[j].OrderStatus == "待处理") {
                                            statuscode = "0"
                                        } else if (databack[j].OrderStatus == "维修中") {
                                            statuscode = "1"
                                        } else if (databack[j].OrderStatus == "已完成") {
                                            statuscode = "2"
                                        } else if (databack[j].OrderStatus == "已中止") {
                                            statuscode = "3"
                                        }
                                        var json = {
                                            "listId": databack[j].OID,
                                            "listNumber": databack[j].SerialNo,
                                            "listTime": databack[j].CreateTime,
                                            "listType": databack[j].MaintenanceType,
                                            "listState": statuscode,
                                            "listLoca": databack[j].DetailLocation
                                        }
                                        _this.lists.push(json)
                                    }, i * 300); //这一行将i*1000改为j*1000也行，并不影响
                                })();
                            }
                        }
                    })
                }
            },
            loaddata1(page) {
                console.log('loaddata1')
                var _this = this;
                var uid = wx.getStorageSync("UID");
                if (uid) {
                    wx.request({
                        url: 'https://hd.xmountguan.com/railway/order.aspx?func=get_user_orders&uid=' + uid + '&orderstatus=1' + '&page=' + this.page1 + '&pagesize=5', //仅为示例，并非真实的接口地址
                        success(res) {

                            // var Things =
                            //
                            console.log(res.data)
                            var databack = res.data
                            var statuscode = ''
                            if (databack.length < 5) {
                                _this.end1 = true
                            } else {
                                _this.end1 = false
                                _this.page1 = parseInt(_this.page1) + 1
                            }
                            if (timeout0)
                                clearTimeout(timeout0)
                            for (var i = 0; i < databack.length; i++) {
                                // for(let i = 0 ; i < 2; i++){
                                // console.log(databack[i]);
                                (function () {
                                    var j = i;
                                    timeout0 = setTimeout(function timer() {
                                        if (databack[j].OrderStatus == "待处理") {
                                            statuscode = "0"
                                        } else if (databack[j].OrderStatus == "维修中") {
                                            statuscode = "1"
                                        } else if (databack[j].OrderStatus == "已完成") {
                                            statuscode = "2"
                                        } else if (databack[j].OrderStatus == "已中止") {
                                            statuscode = "3"
                                        }
                                        var json = {
                                            "listId": databack[j].OID,
                                            "listNumber": databack[j].SerialNo,
                                            "listTime": databack[j].CreateTime,
                                            "listType": databack[j].MaintenanceType,
                                            "listState": statuscode,
                                            "listLoca": databack[j].DetailLocation
                                        }
                                        _this.lists1.push(json)
                                    }, i * 300); //这一行将i*1000改为j*1000也行，并不影响
                                })();
                            }
                        }
                    })
                }
            },
            loaddata2(page) {
                var _this = this;
                var uid = wx.getStorageSync("UID");
                if (uid) {
                    wx.request({
                        url: 'https://hd.xmountguan.com/railway/order.aspx?func=get_user_orders&uid=' + uid + '&orderstatus=2' + '&page=' + this.page2 + '&pagesize=5', //仅为示例，并非真实的接口地址
                        success(res) {

                            // var Things =
                            //
                            console.log(res.data)
                            var databack = res.data
                            var statuscode = ''
                            if (databack.length < 5) {
                                _this.end2 = true
                            } else {
                                _this.end2 = false
                                _this.page2 = parseInt(_this.page2) + 1
                            }
                            if (timeout0)
                                clearTimeout(timeout0)
                            for (var i = 0; i < databack.length; i++) {
                                // for(let i = 0 ; i < 2; i++){
                                // console.log(databack[i]);
                                (function () {
                                    var j = i;

                                    timeout0 = setTimeout(function timer() {
                                        if (databack[j].OrderStatus == "待处理") {
                                            statuscode = "0"
                                        } else if (databack[j].OrderStatus == "维修中") {
                                            statuscode = "1"
                                        } else if (databack[j].OrderStatus == "已完成") {
                                            statuscode = "2"
                                        } else if (databack[j].OrderStatus == "已中止") {
                                            statuscode = "3"
                                        }
                                        var json = {
                                            "listId": databack[j].OID,
                                            "listNumber": databack[j].SerialNo,
                                            "listTime": databack[j].CreateTime,
                                            "listType": databack[j].MaintenanceType,
                                            "listState": statuscode,
                                            "listLoca": databack[j].DetailLocation
                                        }
                                        _this.lists2.push(json)
                                    }, i * 300); //这一行将i*1000改为j*1000也行，并不影响
                                })();
                            }
                        }
                    })
                }
            },
            loaddata3(page) {
                var _this = this;
                var uid = wx.getStorageSync("UID");
                if (uid) {
                    wx.request({
                        url: 'https://hd.xmountguan.com/railway/order.aspx?func=get_user_orders&uid=' + uid + '&orderstatus=3' + '&page=' + this.page3 + '&pagesize=5', //仅为示例，并非真实的接口地址
                        success(res) {

                            // var Things =
                            //
                            console.log(res.data)
                            var databack = res.data
                            var statuscode = ''
                            if (databack.length < 5) {
                                _this.end3 = true
                            } else {
                                _this.end3 = false
                                _this.page3 = parseInt(_this.page3) + 1
                            }
                            if (timeout0)
                                clearTimeout(timeout0)
                            for (var i = 0; i < databack.length; i++) {
                                // for(let i = 0 ; i < 2; i++){
                                // console.log(databack[i]);
                                (function () {
                                    var j = i;

                                    timeout0 = setTimeout(function timer() {
                                        if (databack[j].OrderStatus == "待处理") {
                                            statuscode = "0"
                                        } else if (databack[j].OrderStatus == "维修中") {
                                            statuscode = "1"
                                        } else if (databack[j].OrderStatus == "已完成") {
                                            statuscode = "2"
                                        } else if (databack[j].OrderStatus == "已中止") {
                                            statuscode = "3"
                                        }
                                        var json = {
                                            "listId": databack[j].OID,
                                            "listNumber": databack[j].SerialNo,
                                            "listTime": databack[j].CreateTime,
                                            "listType": databack[j].MaintenanceType,
                                            "listState": statuscode,
                                            "listLoca": databack[j].DetailLocation
                                        }
                                        _this.lists3.push(json)
                                    }, i * 300); //这一行将i*1000改为j*1000也行，并不影响
                                })();
                            }
                        }
                    })
                }
            },
            loaddata4(page) {
                var _this = this;
                var uid = wx.getStorageSync("UID");
                if (uid) {
                    wx.request({
                        url: 'https://hd.xmountguan.com/railway/order.aspx?func=get_user_orders&uid=' + uid + '&orderstatus=4' + '&page=' + this.page4 + '&pagesize=5', //仅为示例，并非真实的接口地址
                        success(res) {

                            // var Things =
                            //
                            console.log(res.data)
                            var databack = res.data
                            var statuscode = ''
                            if (databack.length < 5) {
                                _this.end4 = true
                            } else {
                                _this.end4 = false
                                _this.page4 = parseInt(_this.page4) + 1
                            }
                            if (timeout0)
                                clearTimeout(timeout0)

                            for (var i = 0; i < databack.length; i++) {
                                // for(let i = 0 ; i < 2; i++){
                                // console.log(databack[i]);
                                (function () {
                                    var j = i;

                                    timeout0 = setTimeout(function timer() {
                                        if (databack[j].OrderStatus == "待处理") {
                                            statuscode = "0"
                                        } else if (databack[j].OrderStatus == "维修中") {
                                            statuscode = "1"
                                        } else if (databack[j].OrderStatus == "已完成") {
                                            statuscode = "2"
                                        } else if (databack[j].OrderStatus == "已中止") {
                                            statuscode = "3"
                                        }
                                        var json = {
                                            "listId": databack[j].OID,
                                            "listNumber": databack[j].SerialNo,
                                            "listTime": databack[j].CreateTime,
                                            "listType": databack[j].MaintenanceType,
                                            "listState": statuscode,
                                            "listLoca": databack[j].DetailLocation
                                        }
                                        _this.lists4.push(json)
                                    }, i * 300); //这一行将i*1000改为j*1000也行，并不影响
                                })();
                            }
                        }
                    })
                }
            },
        },
        onload() {
            console.log('on load')
        },
        mounted() {
            console.log('mounted')
            let _this = this;
            let wx = mpvue;
            setTimeout(() => {
                _this.user = wx.getStorageSync("user")
                _this.login = wx.getStorageSync('UID')
                console.log(_this.login);


                if (_this.login) {
                    console.log('加载数据')
                    _this.loaddatas()
                }
            }, 1500)
            //roel init
            // var Role = wx.getStorageSync('Role');
            // if(Role == 1){
            //     _this.navtabs.splice(1,1)
            // }
            // this.Role = Role
            // let app = getApp()
            mpvue.getSystemInfo({
                success: function (res) {
                    var w = res.windowWidth
                    var h = res.windowHeight
                    var calHeight = (h / w * 750 - 200).toFixed(2)
                    _this.scrollData.height = calHeight + "rpx"
                    console.log(_this.scrollData.height);
                },
            })

            // if(!login){
            //     wx.redirectTo({
            //         // url: '../detailforworker/main',
            //         url : '../login/main',
            //     })
            // }
            wx.showShareMenu();
            // _this.loaddata(this.page)
            // _this.loaddata1(this.page1)
            // _this.loaddata2(this.page2)
            // _this.loaddata3(this.page3)
            // _this.loaddata4(this.page4)
        },
        onPullDownRefresh: function () {
            // //直接获取到当前页面的onload()进行刷新
            // var that = this
            // // that.onload()
            // wx.showToast({
            //     title: '下拉。。。。。。',
            //     icon: 'none',
            //     duration: 2000
            // })
            // wx.stopPullDownRefresh()
        },
        onShow() {
            /* var stateChange = wx.getStorageSync('stateChange');
             if(stateChange !== ""){
                 this.lists[wx.getStorageSync('cardinex')].listState = stateChange
                 wx.setStorageSync('cardinex','');
                 wx.setStorageSync('stateChange','');
             }*/
            console.log('show')
            // this.loaddatas()
        },
        // 在app.js里写下以下代码
        /*  onLaunch(){
			  console.log('onLaunch')
			  var wx = mpvue
			  if(wx.canIUse('getUpdateManager')){
				  const updateManager = wx.getUpdateManager()
				  updateManager.onCheckForUpdate(function(res){
					  console.log('onCheckForUpdate====',res)
					  // 请求完新版本信息的回调
					  if(res.hasUpdate){
						  console.log('res.hasUpdate====')
						  updateManager.onUpdateReady(function(){
							  wx.showModal({
								  title  : '更新提示',
								  content: '新版本已经准备好，是否重启应用？',
								  success: function(res){
									  console.log('success====',res)
									  // res: {errMsg: "showModal: ok", cancel: false, confirm: true}
									  if(res.confirm){
										  // 新的版本已经下载好，调用 applyUpdate 应用新版本并重启
										  updateManager.applyUpdate()
									  }
								  }
							  })
						  })
						  updateManager.onUpdateFailed(function(){
							  // 新的版本下载失败
							  wx.showModal({
								  title  : '已经有新版本了哟~',
								  content: '新版本已经上线啦~，请您删除当前小程序，重新搜索打开哟~'
							  })
						  })
					  }
				  })
			  }
		  }*/
    }
</script>
<style scoped>
    .box button::after {
        border: none;
    }

    .gray {
        color: gray;
    }

    .yellow {
        color: #ff8c2e;
    }

    .green {
        color: green;
    }

    .red {
        color: #ed283c;
    }

    .miantitle {
        width: 100%;
        display: block;
        text-align: center;
        font-size: 35rpx;
        position: absolute;
        top: 22rpx;
        left: 0;
    }

    .demo-container {
        width: 400rpx;
        height: 100vh;
        background: white;
        display: flex;
        display: -webkit-flex;
        justify-content: flex-start;
        align-items: center;
        flex-flow: column wrap;
        padding-top: 118rpx;
    }

    .i-drawer-mask {
        background: rgba(28, 36, 56, 0.29);
    }

    .tabs {
        width: 92rpx;
        font-size: 20rpx;
    }

    .icon {
        position: fixed;
        left: 36rpx;
        top: -1rpx;
        z-index: 1;
    }

    .itabs {
        position: fixed;
        z-index: 2;
        width: 648rpx;
        box-sizing: border-box;
        top: 0;
        left: 102rpx;
        justify-content: space-around;
        display: block;
    }

    .usericon {
        width: 112rpx;
        height: 112rpx;
        /*border:2rpx solid #08a7e2; */
        border-radius: 100%;
        -webkit-border-radius: 100%;
    }

    .username {
        display: block;
        margin-top: 20rpx;
        font-size: 28rpx;
        color: #595959;
    }

    .cz {
        height: 50rpx;
        line-height: 50rpx;
        width: 80%;
        margin-top: 40rpx;
        font-size: 28rpx;
        color: #595959;
    }

    .chongzhiCell {
        margin-top: 20rpx;
    }

    .card {
        display: block;
        width: 730rpx;
        height: 320rpx;
        /*background: #595959;*/
        margin: 0 auto;
        margin-bottom: 10rpx;
        border: 1rpx solid rgba(128, 128, 128, 0.53);
        position: relative;
        border-radius: 10rpx;
        -webkit-border-radius: 10rpx;
        background: white;
        position: relative;
    }

    .tabscontent {
        display: flex;
        justify-content: center;
        flex-flow: column wrapper;
    }

    .danhao {
        position: absolute;
        left: 31rpx;
        top: 28rpx;
        font-size: 28rpx;
    }

    .state {
        position: absolute;
        right: 31rpx;
        top: 28rpx;
        font-size: 28rpx;
    }

    .listtime,
    .listtype,
    .listlocation {
        position: absolute;
        left: 26rpx;
        top: 86rpx;
        display: block;
        font-size: 32rpx;
        line-height: 40rpx;
    }

    .listtype {
        top: 162rpx;
    }

    .listlocation {
        top: 232rpx;
    }

    .listicon {
        font-size: 29px;
        color: #ACACAC;
        position: relative;
        top: 4rpx;
    }

    .tdetail {
        margin-left: 10rpx;
        font-size: 28rpx;
    }

    .listlocation .tdetail {
        height: 40rpx;
        width: 612rpx;
        display: inline-block;
        overflow: hidden;
        position: relative;
        left: 0;
        top: 10rpx;
        word-break: break-all;

    }

    .swiper {
        width: 100vw;
        display: block;
        overflow: hidden;
        height: 100vh;
        position: fixed;
        left: 0;
        top: 0;
        padding-top: 85rpx;
        box-sizing: border-box;
    }

    .swiperitem {
        width: 100%;
        height: 100%;
        overflow-x: hidden;
        overflow-y: hidden;
        display: block;
        position: relative;
        padding-bottom: 30rpx;
    }

    .goreport {
        width: 100%;
        position: fixed;
        left: 0;
        bottom: 0;
    }

    .freshicon {
        position: absolute;
        width: 750rpx;
        left: 0;
        text-align: center;
        display: block;
        font-size: 20rpx;
        color: rgba(0, 0, 0, 0.56);
        margin-top: 10rpx;
    }

    .nodata {
        width: 100%;
        height: 100rpx;
        font-size: 20rpx;
        color: rgba(0, 0, 0, 0.62);
        text-align: center;
        line-height: 100rpx;
    }
</style>
