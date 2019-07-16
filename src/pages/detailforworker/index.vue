<template>
    <div style="padding-bottom:150rpx;">
        <div class="ipts">
            <span class="ititle">报修单号</span>
            <span class="ript">{{detail.origin.danhao}}</span>
        </div>
        <div class="ipts">
            <span class="ititle">状态</span>
            <span
                :class="[{  gray  : detail.state=='0' },{  yellow  : detail.state=='1' },{  green  : detail.state=='2' },{  red  : detail.state=='3' }, 'ript']">{{detail.state==0?"待处理":(detail.state==1?"维修中":(detail.state==2?"已完成":"已中止"))}}</span>
        </div>
        <div class="ipts">
            <span class="ititle">报修人姓名</span>
            <span class="ript">{{detail.origin.name}}</span>
        </div>
        <div class="ipts">
            <span class="ititle">手机号</span>
            <span class="ript phone" @click='makeacall' :data-cell="detail.origin.phone">{{detail.origin.phone}}</span>
        </div>
        <div class="ipts">
            <span class="ititle">报修类型</span>
            <span class="ript ">{{detail.origin.type}}</span>
        </div>
        <div class="ipts">
            <span class="ititle">报修内容</span>
            <span class="ript riptcontent">{{detail.origin.content[0]}}</span>
        </div>
        <div class="ipts">
            <span class="ititle imgs ">现场图片:</span>
            <div style="margin-left:45rpx;width:auto;">
                <div class="imgbox" v-for="(item,index) in detail.origin.imgsUrl" :key="index">
                    <img :src="item" :mode="'widthFix'" @click='preview(index)' class="slt" alt="缩略图">
                </div>
            </div>
        </div>
        <div class="ipts">
            <span class="ititle">车站</span>
            <div class="loca">
                <i-icon style="position:relative;top:-4rpx;"
                        type="coordinates_fill" size="26"
                        color="#2d8cf0"
                        class="usericon"/>
                <span class="spans  ">{{detail.origin.station}}</span>
            </div>
        </div>
        <div class="ipts">
            <span class="ititle" style="float: none;">详细位置描述</span>
            <span class="address addressworker">{{detail.origin.address }}</span>
        </div>
        <!--   <div class="ipts">
               <span class="ititle">台单号：</span>
               <span class="ript">{{detail.origin.taidanhao}}</span>
           </div>
          -->
        <i-row :class="'buttons'">
            <i-col :span="16">
                <i-button type="primary" @click="jiedan">接 单</i-button>
            </i-col>
            <i-col :span="8">
                <i-button type="warning" @click="fankui">反 馈</i-button>
            </i-col>
        </i-row>
        <view class="paidan">
            <view class="paidancard">
                <span class="cardtitle">派单</span>
                <span class="cardline">
                    <span class="linetitel">*维修人员</span>
                    <span class="linedetail" @click.stop="selectPeople">>请选择</span>
                </span>
                <span class="cardline" style="margin-bottom:75rpx;">
                    <span class="linetitel">*到场时间</span>
                    <span class="linedetail" @click.stop.click="dateTimePick">>请选择</span>
                </span>

                <view class="sure">
                    <i-button type="primary">确认派单</i-button>
                </view>
            </view>
            <van-popup :show="datepickershow" position="bottom">

                <van-datetime-picker :type="'datetime'" :data-value="currentDate" :value="currentDate"
                                     v-model="currentDate" :min-date="minDate" @confirm="confirm" @cancel="cancel"/>
            </van-popup>
        </view>

        <i-toast id="toast"/>
    </div>
</template>
<script>
    import {$Toast} from '../../../static/iview/base/index'

    export default {
        data() {
            return {
                minHour: 10,
                maxHour: 20,
                minDate: new Date().getTime(),
                maxDate: new Date(2019, 10, 1).getTime(),
                currentDate: new Date().getTime(),
                detail: {
                    state: 0,
                    banzu: '一工队',
                    gongzhang: '张三',
                    gzcell: '13858585654',
                    weixiugong: '李四',
                    wxgcell: '13865656545',
                    jubao: '10086',
                    location: '',
                    origin: {
                        danhao: 'WX1245151424',
                        time: "2019年7月04日 18:44",
                        name: "张三",
                        phone: "13854587485",
                        type: '水电问题',
                        content: ['度数不转', '水表度数块', '其他问题', '还有问题', '能量不打钩', '旖旎你水电'],
                        imgsUrl: ['http://www.simpleqq.com/index/imgs/tab/tab4.jpg', "https://www.simpleqq.com/index/imgs/imgdemo.jpg"],
                        station: "杭州南站",
                        address: "杭州南站习广场东侧候车厅小隔间大阳台小浴室的拐角的洞洞里",
                        taidanhao: 'this is off'
                    },

                },
                selectIndex: '',
                judgeShow: false,
                reasonShow: false,
                badReason: "",
                datepickershow: false

            }
        },
        computed: {
            judgement: function () {
                console.log(this.detail.state);
                return (this.detail.state == 2 || this.detail.state == 3) ? true : false
            }
        },
        methods: {
            makeacall(e) {
                let wx = mpvue
                let number = e.mp.currentTarget.dataset.cell
                console.log(number)
                wx.makePhoneCall({
                    phoneNumber: number //仅为示例，并非真实的电话号码
                })
            },

            preview: function (key) {
                wx.previewImage({
                    current: this.detail.origin.imgsUrl[key], // 当前显示图片的http链接
                    urls: this.detail.origin.imgsUrl  // 需要预览的图片http链接列表
                })
            },
            jiedan: () => {
                console.log("jiedan");
            },
            fankui: () => {
                console.log("fankui");
            },
            selectPeople: function () {
                console.log("选择人员")
                this.datepickershow = true
            },
            dateTimePick: function () {
                console.log("dateTimePick")
                this.datepickershow = true
            },
            confirm: function (e) {

                console.log(e);
                var timestamp=e.mp.currentTarget.dataset.value
                // console.log(this.formatDate(timestamp))
                this.datepickershow = !this.datepickershow
            },
            cancel: function () {
                console.log("pickerclose");
                this.datepickershow = !this.datepickershow

            },
            formatDate: function (now) {
                now=now/1000
                var year = now.getFullYear();
                var month = now.getMonth() + 1;
                var date = now.getDate();
                var hour = now.getHours();
                var minute = now.getMinutes();
                var second = now.getSeconds();
                return year + "-" + month + "-" + date + " " + hour + ":" + minute + ":" + second;
            }

        },
        created() {
        },
        onShow: function () {
        },
    }
</script>
<style>
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

    .ipts {
        width: 740rpx;
        min-height: 38px;
        line-height: 70rpx;
        font-size: 28rpx;
        display: block;
        margin: 0 auto;
        color: #3f4147;
        border-bottom: 1rpx solid rgba(222, 222, 222, 0.67);

    }

    .ititle {
        display: block;
        float: left;
        padding-left: 10rpx;
    }

    .loca {
        float: right;
    }

    .imgs {
        float: none;
    }

    .ript {
        display: block;
        height: 70rpx;
        line-height: 70rpx;
        float: right;
        text-align: right;
        padding-right: 10rpx;
        /*background: #ededed;*/
        width: 525rpx;

    }

    .riptcontent {
        display: block;
        height: 56rpx;
        line-height: 56rpx;
        float: right;
        text-align: right;
        padding-right: 10rpx;
        width: auto;
        border: 1rpx solid rgba(63, 65, 71, 0.31);
        padding: 0 10rpx;
        border-radius: 10rpx;
        vertical-align: middle;
        margin-top: 10rpx;

    }

    .phone {
        color: #2d8cf0;
    }

    .active .gou {
        display: inline;
    }

    .detailbox {
        width: 100%;
        padding: 20rpx;
        box-sizing: border-box;
        -webkit-box-sizing: border-box;

    }

    .xqtxt {
        font-weight: bold;
    }

    .detail {
        width: 100%;
        min-height: 600rpx;
        border: 1rpx solid rgba(128, 128, 128, 0.37);
        margin-top: 10rpx;
        padding: 10rpx;
        box-sizing: border-box;
        box-shadow: 0 0 10rpx rgba(92, 92, 92, 0.36);

    }

    .bxlist {
        width: 100%;
        display: block;
        min-height: 50rpx;
        line-height: 50rpx;
        font-size: 28rpx;
        text-align: justify;
        padding-left: 10rpx;
        box-sizing: border-box;
    }

    .spans, .neirong {
        display: inline-block;
    }

    .neirong {
        border: 1rpx solid #b2b8c5;
        border-radius: 10rpx;
        margin: 0 10rpx 10rpx 10rpx;
        box-sizing: border-box;
        padding: 0 10rpx;
        font-size: 24rpx;

    }

    .inspan {
        margin-right: 20rpx;
    }

    .imgbox {
        display: inline-block;
        width: 200rpx;
        height: 200rpx;
        background: rgba(170, 255, 119, 0.6);
        margin: 0 20rpx 0 0;
        position: relative;
        overflow: hidden;
    }

    .slt {
        position: absolute;
        width: 100%;
        height: auto;
        display: block;
        left: 0;
        top: 50%;
        transform: translateY(-50%);
        -webkit-transform: translateY(-50%);
    }

    .judge {
        margin-bottom: 150rpx;
    }

    .addressworker {
        display: block;
        float: none;
        background: #f7f4f4;
        width: 91%;
        margin: 0 auto;
        margin-bottom: 21rpx;
        line-height: 40rpx;
        padding: 10rpx;
        text-align: justify;

    }

    .paidan {
        width: 100vw;
        height: 100vh;
        position: fixed;
        left: 0;
        top: 0;
        z-index: 1000;
        background: rgba(128, 128, 128, 0.78);
    }

    .paidancard {
        width: 75%;
        min-height: 522rpx;
        background: white;
        border-radius: 20rpx;
        -webkit-border-radius: 20rpx;
        position: absolute;
        left: 50%;
        top: 50%;
        transform: translate(-50%, -50%);
        -webkit-transform: translate(-50%, -50%);
    }

    .cardtitle {
        width: 100%;
        margin: 0 auto;
        text-align: center;
        display: block;
        margin-top: 29rpx;
        margin-bottom: 37rpx;

    }

    .cardline {
        display: block;
        margin: 0 auto;
        width: 75%;
        border-bottom: 1rpx solid rgba(128, 128, 128, 0.5);
        overflow: hidden;
        height: 75rpx;
        line-height: 75rpx;
        font-size: 24rpx;
        margin-bottom: 18rpx;
    }

    .linetitel {
        float: left;
    }

    .linedetail {
        text-align: right;
        float: right;
    }

    .sure {
        margin-top: 40rpx;
        width: 84%;
        margin: 0 auto;

    }

</style>
