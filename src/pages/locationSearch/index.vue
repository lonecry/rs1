<template>
    <div>
        <van-search v-model="value" placeholder="请输入搜索关键词" @change="vchange" @search="onSearch" @cancel="onCancel"/>

        <div class="stationlist" :style="[{height: stationheight }]">
            <span v-if="noresult"> 没有结果！</span>
            <span v-if="!noresult" class="notes">你是不是这个车站？</span>
            <div v-if="!noresult" class="list1">
                <span :class="[{loactive:index==activelocation}]" @click='choselocation'
                      :data-skey="index" :data-sid="item.sid" :data-sname="item.sname" v-for="(item,index) in locations"
                      :key=" index" class="stations" v-if="index==0">
                    <i-icon type="coordinates_fill" size="21" class=" locaiocn "/>{{item.sname}} </span>
            </div>
            <span v-if="!noresult" class="notes">其他附近车站</span>
            <div v-if="!noresult" class="list1">
                <span :class="[{loactive:index==activelocation}]" @click='choselocation'
                      :data-skey="index" :data-sid="item.sid" :data-sname="item.sname" v-for="(item,index) in locations"
                      :key=" index" class="stations" v-if="index">
                    <i-icon type="coordinates_fill" size="21" class=" locaiocn "/>{{item.sname}}</span>
                <span class="searchtiips">找不到？试试搜索吧~</span>
            </div>
        </div>
        <i-toast id="toast"/>
    </div>
</template>
<script>
    import {$Toast} from '../../../static/iview/base/index'

    export default {
        components: {},
        data() {
            return {
                stationheight: '',
                value: "杭州",
                activelocation: -1,
                location: '',
                locations: [],
                sid: '',
                sname: '',
                noresult: false
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

                var _this = this
                wx.request({

                    url: 'https://hd.xmountguan.com/railway/station.aspx?func=get_wx_station_ddl&key=' + this.value,

                    success(res) {
                        _this.locations = []
                        if (res.data.length > 0) {
                            _this.noresult = false
                        } else {
                            _this.noresult = true
                        }


                        for (let item of res.data) {
                            _this.locations.push(item)
                        }
                        console.log(_this.locations);


                    }
                })

                //
                // if (this.value) {
                //     $Toast({
                //         content: this.value,
                //         // type: 'warning'
                //     });
                // }
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
                var _this = this
                var addr = e.mp.currentTarget.dataset.loname
                console.log(addr);
                _this.activelocation = e.mp.currentTarget.dataset.skey
                _this.sid = e.mp.currentTarget.dataset.sid
                _this.sname = e.mp.currentTarget.dataset.sname
                console.log(_this.sid, _this.sname)
                setTimeout(() => {
                    mpvue.redirectTo({
                        // ./join/main
                        url: '../report/main?fromreport=yes&sid=' + _this.sid + '&sname=' + _this.sname,
                    })
                }, 300)
                mpvue.setStorageSync('addr', addr)
                // mpvue.navigateBack()
            }
        },
        mounted() {
            var wx = mpvue
            // {"latitude":30.18534,"longitude":120.26457,"speed":-1,"accuracy":65,"verticalAccuracy":65,"horizontalAccuracy":65,"errMsg":"getLocation:ok"}
            var longitude = wx.getStorageSync('location').longitude//经度

            var latitude = wx.getStorageSync('location').latitude//维度
            console.log(latitude, longitude)

            var _this = this
            _this.value = "杭州",
                wx.request({
                    // https://hd.xmountguan.com/railway/station.aspx?func=get_nearest_station&xcoordinate=120.219396&ycoordinate=30.305972
                    url: 'https://hd.xmountguan.com/railway/station.aspx?func=get_nearest_station&xcoordinate=' + longitude + '&ycoordinate=' + latitude, //仅为示例，并非真实的接口地址

                    success(res) {
                        _this.locations = []
                        if (res.data.length > 0) {
                            _this.noresult = false
                        } else {
                            _this.noresult = true
                        }


                        for (let item of res.data) {
                            _this.locations.push(item)
                        }
                        console.log(_this.locations);


                    }
                })
        },
        onShow() {
            let _this = this;
            // let app = getApp()
            mpvue.getSystemInfo({
                success: function (res) {
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
