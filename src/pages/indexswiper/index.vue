<template>
    <div class = " box">
        <i-icon type = "mine" @click = "toggleLeft" size = "26" color = "#ACACAC" class = "icon"/>
        <i-tabs class = 'itabs' :current = "current_scroll" @change = "handleChangeScroll">
            <i-tab key = "0" class = "tabs" :title = "'全部'"></i-tab>
            <i-tab key = "1" class = "tabs" :title = "'待处理'"></i-tab>
            <i-tab key = "2" class = "tabs" :title = "'维修中'"></i-tab>
            <i-tab key = "3" class = "tabs" :title = "'已完成'"></i-tab>
            <i-tab key = "4" class = "tabs" :title = "'已中止'"></i-tab>
        </i-tabs>
        <swiper class = "swiper"
                duration = "300"
                :current = "current_scroll"
                @change = "change"
        >
            <swiper-item class = "swiperitem">
                <scroll-view scroll-y = "true" :style = "computedClassObject">
                    <div class = "card" :data-cardid = "item.listId" ref = "dataNum" @click = "cardClick($event)" v-for = "(item,index) in   listData.lists" :key = "index">
                        <span class = "danhao">保修单号:{{item.listNumber}}</span>
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
                <scroll-view :scroll-y = "scrolly" :style = "computedClassObject">
                    <div class = "card" :data-cardid = "item.listId" ref = "dataNum" @click = "cardClick($event)" v-for = "(item,index) in   listData.lists" :key = "index" v-if = "item.listState==0">
                        <span class = "danhao">保修单号:{{item.listNumber}}</span>
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
                <scroll-view :scroll-y = "scrolly" :style = "computedClassObject">
                    <div class = "card" :data-cardid = "item.listId" ref = "dataNum" @click = "cardClick($event)" v-for = "(item,index) in   listData.lists" :key = "index" v-if = "item.listState==1">
                        <span class = "danhao">保修单号:{{item.listNumber}}</span>
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
                <scroll-view :scroll-y = "scrolly" :style = "computedClassObject">
                    <div class = "card" :data-cardid = "item.listId" ref = "dataNum" @click = "cardClick($event)" v-for = "(item,index) in   listData.lists" :key = "index" v-if = "item.listState==2">
                        <span class = "danhao">保修单号:{{item.listNumber}}</span>
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
                <scroll-view :scroll-y = "scrolly" :style = "computedClassObject">
                    <div class = "card" :data-cardid = "item.listId" ref = "dataNum" @click = "cardClick($event)" v-for = "(item,index) in   listData.lists" :key = "index" v-if = "item.listState==3">
                        <span class = "danhao">保修单号:{{item.listNumber}}</span>
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
        <i-drawer mode = "left" @click = "toggleLeft" :visible = "showleft" class = "i-drawer-mask">
            <div class = "demo-container" @click.stop>
                <img :src = "userIcon" class = "usericon" @click = "changeIcon" alt = ""/>
                <span class = "username">{{usserName}}</span>
                <view class = "cz chongzhiPsw">
                    <i-icon type = "lock_fill" size = "21" color = "#ACACAC"/>
                    重置密码
                </view>
                <view class = "cz chongzhiCell">
                    <i-icon type = "mobilephone_fill" size = "21" color = "#ACACAC"/>
                    更换绑定手机号
                </view>
            </div>
        </i-drawer>
    </div>
</template>
<script>
    export default {
        data(){
            return {
                showleft: false,
                current_scroll: '0',
                userIcon: '/static/images/Avator.png',
                usserName: "一万年朝夕",
                scrolly: true,
                scrollData: {
                    height: '1100rpx',
                    "-webkit-overflow-scrolling": "touch",
                },
                listData: {
                    lists: [
                        {
                            "listId": 'l2213213',
                            "listNumber": 'WX2019090920',
                            "listTime": '7月8日 18:09',
                            "listType": '水电问题',
                            "listState": '0',
                            "listLoca": "杭州东站东区西广场南候车厅北侧"
                        }, {
                            "listId": 'l2137613',
                            "listNumber": 'WX2019090920',
                            "listTime": '7月8日 18:09',
                            "listType": '水电问题',
                            "listState": '0',
                            "listLoca": "杭州东站东区西广场南候车厅北侧"
                        }, {
                            "listId": 'l212313',
                            "listNumber": 'WX201320920',
                            "listTime": '7月8日 18:09',
                            "listType": '土建问题',
                            "listState": '1',
                            "listLoca": "绍兴东站东区西广场南候车厅北侧"
                        }, {
                            "listId": 'l256213',
                            "listNumber": 'WX203590920',
                            "listTime": '7月8日 18:09',
                            "listType": '系统故障',
                            "listState": '2',
                            "listLoca": "宁波东站东区西广场南候车厅北侧"
                        }, {
                            "listId": 'l256213',
                            "listNumber": 'WX203590920',
                            "listTime": '7月8日 18:09',
                            "listType": '系统故障',
                            "listState": '2',
                            "listLoca": "宁波东站东区西广场南候车厅北侧"
                        }, {
                            "listId": 'l256213',
                            "listNumber": 'WX203590920',
                            "listTime": '7月8日 18:09',
                            "listType": '系统故障',
                            "listState": '2',
                            "listLoca": "宁波东站东区西广场南候车厅北侧"
                        }, {
                            "listId": 'l256213',
                            "listNumber": 'WX203590920',
                            "listTime": '7月8日 18:09',
                            "listType": '系统故障',
                            "listState": '2',
                            "listLoca": "宁波东站东区西广场南候车厅北侧"
                        }, {
                            "listId": 'l256213',
                            "listNumber": 'WX203590920',
                            "listTime": '7月8日 18:09',
                            "listType": '系统故障',
                            "listState": '2',
                            "listLoca": "宁波东站东区西广场南候车厅北侧"
                        }, {
                            "listId": 'l256213',
                            "listNumber": 'WX203590920',
                            "listTime": '7月8日 18:09',
                            "listType": '系统故障',
                            "listState": '2',
                            "listLoca": "宁波东站东区西广场南候车厅北侧"
                        }, {
                            "listId": 'l256213',
                            "listNumber": 'WX203590920',
                            "listTime": '7月8日 18:09',
                            "listType": '系统故障',
                            "listState": '2',
                            "listLoca": "宁波东站东区西广场南候车厅北侧"
                        }, {
                            "listId": 'l256213',
                            "listNumber": 'WX203590920',
                            "listTime": '7月8日 18:09',
                            "listType": '系统故障',
                            "listState": '2',
                            "listLoca": "宁波东站东区西广场南候车厅北侧"
                        }, {
                            "listId": 'l256213',
                            "listNumber": 'WX203590920',
                            "listTime": '7月8日 18:09',
                            "listType": '系统故障',
                            "listState": '2',
                            "listLoca": "宁波东站东区西广场南候车厅北侧"
                        }, {
                            "listId": 'l219343',
                            "listNumber": 'WX2013290920',
                            "listTime": '7月19日 08:09',
                            "listType": '信号故障',
                            "listState": '3',
                            "listLoca": "宁波东站东区西广场南候车厅北侧"
                        },
                    ],
                },
            }
        },
        components: {},
        computed: {
            classObject: function (){
                return {
                    active: this.isActive,
                    sort: this.isSort,
                }
            },
            computedClassObject(){
                return this.showJson({
                    height: this.scrollData.height,
                })
            }
        },
        methods: {
            showJson(style){
                let s = []
                for (let i in style) {
                    s.push(i + ':' + style[i]);
                }
                s = s.join(';')
                return s
            },
            toggleLeft(e){
                this.showleft = !this.showleft
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
            cardClick(e){
                // console.log(e.currentTarget.dataset.cardid);
                // console.log(e.target);
                console.log("cardid is " + e.mp.currentTarget.dataset.cardid);
            },
            change(e){
                this.current_scroll = e.mp.detail.current
            }
        },
        onShow(){
            let _this = this;
            // let app = getApp()
            mpvue.getSystemInfo({
                success: function (res){
                    /* console.log(res.windowWidth);*/
                    /* console.log(res.windowHeight);*/
                    var w = res.windowWidth
                    var h = res.windowHeight
                    var calHeight = (h / w * 750 - 100).toFixed(2)
                    _this.scrollData.height = calHeight + "rpx"
                    console.log(_this.scrollData.height);
                },
            })
        },
    }
</script>
<style scoped>
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
        top      : 21rpx;
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
    .listtime, .listtype, .listlocation {
        position    : absolute;
        left        : 26rpx;
        top         : 86rpx;
        display     : block;
        font-size   : 32rpx;
        line-height : 40rpx;

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
</style>
