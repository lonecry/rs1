<template>
    <div class = " box">
        <i-tabs class = 'itabs' :current = "current_scroll" scroll @change = "handleChangeScroll">
            <i-icon type = "mine" @click = "toggleLeft" size = "26" color = "#ACACAC" class = "icon"/>
            <i-tab key = "0" class = "tabs" :title = "'    '"></i-tab>
            <i-tab key = "1" class = "tabs" :title = "'全部'"></i-tab>
            <i-tab key = "2" class = "tabs" :title = "'待处理'"></i-tab>
            <i-tab key = "3" class = "tabs" :title = "'维修中'"></i-tab>
            <i-tab key = "4" class = "tabs" :title = "'已完成'"></i-tab>
            <i-tab key = "5" class = "tabs" :title = "'已中止'"></i-tab>
        </i-tabs>
        <div class = "tabscontent">
            <div v-show = "current_scroll==1" class = "tcontent">
                <div class = "card"  :data-cardid = "item.listId" ref = "dataNum" @click = "cardClick($event)" v-for = "(item,index) in   listData.lists" :key = "index">
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
            </div>
            <div v-show = "current_scroll==2" class = "tcontent">内容二</div>
            <div v-show = "current_scroll==3" class = "tcontent">内容三</div>
            <div v-show = "current_scroll==4" class = "tcontent">内容四</div>
            <div v-show = "current_scroll==5" class = "tcontent">内容⑤</div>
        </div>
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
                current_scroll: '1',
                userIcon: '/static/images/Avator.png',
                usserName: "一万年朝夕",
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
                            "listId": 'l219343',
                            "listNumber": 'WX2013290920',
                            "listTime": '7月19日 08:09',
                            "listType": '信号故障',
                            "listState": '3',
                            "listLoca": "宁波东站东区西广场南候车厅北侧"
                        },
                    ],
                }
            }
        },
        components: {},
        computed: {
            classObject: function (){
                return {
                    active: this.isActive,
                    sort: this.isSort
                }
            }
        },
        methods: {
            toggleLeft(e){
                console.log(' draw true')
                console.log(e)
                this.showleft = !this.showleft
                this.current_scroll = '1'
            },
            handleChangeScroll(e){
                console.log(e.target.key);
                this.current_scroll = e.target.key;
                if (this.current_scroll == '0')
                    this.toggleLeft()
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
                console.log("cardid is "+e.mp.currentTarget.dataset.cardid);
            }
        },
        created(){
            // let app = getApp()
        }
    }
</script>
<style scoped>
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

    }

    .i-drawer-mask {
        background: rgba(28, 36, 56, 0.29);
    }

    .tabs {
        width: 200rpx;
        font-size: 20rpx;
    }

    .icon {
        position: absolute;
        left: 38rpx;
        top: 0rpx;
        z-index: 1;

    }

    .itabs {
        position: relative;
        z-index: 2;
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
        font-size: 35rpx;
        color: #595959;

    }

    .cz {
        height: 50rpx;
        line-height: 50rpx;
        width: 80%;
        margin-top: 40rpx;
        font-size: 35rpx;
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

    .listtime, .listtype, .listlocation {
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

    }
</style>
