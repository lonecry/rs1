<template>
    <div>
        <van-search v-model="value" placeholder="请输入搜索关键词" @change="vchange" @search="onSearch" @cancel="onCancel" />
        <div class="stationlist" :style="[{height: stationheight }]">
            <span class="notes">你是不是这个车站？</span>
            <div class="list1">
                <span :class="[{loactive:item.loindex==activelocation}]" @click='choselocation' :data-locaid="item.loindex" :data-loname="item.location" v-for="(item,index) in locations[0]" :key=" index" class="stations">
                    <i-icon type="coordinates_fill" size="21" class=" locaiocn " />{{item.location}}<span class="gou">√</span></span>
            </div>
            <span class="notes">其他附近车站</span>
            <div class="list1">
                <span :class="[{loactive:item.loindex==activelocation}]" @click='choselocation' :data-locaid="item.loindex" :data-loname="item.location" v-for="(item,index) in locations[1]" :key=" index" class="stations">
                    <i-icon type="coordinates_fill" size="21" class=" locaiocn " />{{item.location}}<span class="gou">√</span></span>
                <span class="searchtiips">找不到？试试搜索吧~</span>
            </div>
        </div>
        <i-toast id="toast" />
    </div>
</template>
<script>
import { $Toast } from '../../../static/iview/base/index'

export default {
    components: {},
    data() {
        return {
            stationheight: '',
            value: "浙江",
            activelocation: '0',
            location: '',
            locations: [
                [{ loindex: 0, location: "杭州东站" }, ],
                [{ loindex: 1, location: "宁波东站" },
                    { loindex: 2, location: "长兴南东站" },
                    { loindex: 3, location: "温州东站" },
                    { loindex: 4, location: "衢州东站" },
                    { loindex: 5, location: "北京西东站" },
                    { loindex: 6, location: "蕲州东站" },
                ]
            ],
        }
    },
    methods: {
        vchange(e) {
            // console.log(e);
            this.value = e.mp.detail
        },
        onSearch() {
            console.log("onSearch")
            console.log(this.value)
            if (this.value) {
                $Toast({
                    content: this.value,
                    // type: 'warning'
                });
            }
        },
        onCancel() {
            console.log("onCancel")
            $Toast({
                content: '请输入手机号码',
                type: 'warning'
            });
        },
        choselocation(e) {
            // console.log(e)
            var addr = e.mp.currentTarget.dataset.loname
            console.log(addr);
            this.activelocation = e.mp.currentTarget.dataset.locaid
            mpvue.redirectTo({
                // ./join/main
                url: '../report/main?addr=' + addr,
            })
            mpvue.setStorageSync('addr', addr)
            // mpvue.navigateBack()
        }
    },
    created() {},
    onShow() {
        let _this = this;
        // let app = getApp()
        mpvue.getSystemInfo({
            success: function(res) {
                console.log(res.windowWidth);
                console.log(res.windowHeight);
                var w = res.windowWidth
                var h = res.windowHeight
                var calHeight = (h / w * 750 - 108).toFixed(2)
                _this.stationheight = calHeight + "rpx"
                console.log(_this.stationheight);
            },
        })
        this.location = wx.getStorageSync('location')
        console.log(this.location);
    },
}

</script>
<style>
.stationlist {
    display: block;
    background: #efefef;
    /*height     : 100vh;*/
    display: block;
    background: #efefef;
    box-sizing: border-box;
    padding: 35rpx;
    font-size: 25rpx;

}

.list1 {
    width: 100%;
    width: 100%;
    /*height     : 150rpx;*/
    margin-top: 20rpx;
    /*margin-bottom : 20rpx;*/

}

.stations {
    width: 30%;
    height: 67rpx;
    border: 1rpx solid #b2b8c5;
    display: inline-block;
    margin: 0 20rpx 0 0;
    text-align: center;
    line-height: 67rpx;
    font-size: 25rpx;
    box-sizing: border-box;
    margin-bottom: 20rpx;
    color: #484848;

}

.locaiocn {
    display: none;
}

.loactive {
    border: 1rpx solid #2d8cf0;
    color: #2d8cf0;
}

.loactive .locaiocn,
.loactive .gou {
    display: inline-block;
}

.searchtiips {
    width: 100%;
    display: block;
    text-align: center;
    margin-top: 100rpx;
    color: #484848;
}

.gou {
    display: none;
}

</style>
