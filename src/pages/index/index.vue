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
            <div v-show = "current_scroll==1" class="tcontent">
                <div class = "card"></div>
                <div class = "card"></div>
                <div class = "card"></div>
                <div class = "card"></div>
                <div class = "card"></div>
                <div class = "card"></div>
            </div>
            <div v-show = "current_scroll==2" class="tcontent">内容二</div>
            <div v-show = "current_scroll==3" class="tcontent">内容三</div>
            <div v-show = "current_scroll==4" class="tcontent">内容四</div>
            <div v-show = "current_scroll==5" class="tcontent">内容⑤</div>
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
                usserName: "一万年朝夕"
            }
        },
        components: {},
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
            }
        },
        created(){
            // let app = getApp()
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
        height: 500rpx;
        background: #595959;
        margin: 0 auto;
        margin-bottom: 10rpx;
    }
    .tabscontent{
        display:flex;
        justify-content:center;
        flex-flow:column wrapper;

    }


</style>
