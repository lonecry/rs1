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
        <div v-if="detail.state==3" class="ipts">
            <span class="ititle">反馈理由</span>
            <span class="ript riptcontent">{{detail.fktype}}</span>
        </div>
        <div v-if="detail.state==3" class="ipts">
            <span class="ititle" style="float: none;">备注</span>
            <span class="  fkbeizhu">{{detail.fkbeizhu }}</span>
        </div>
        <div v-if="detail.state==1" class="ipts">
            <span class="ititle">维修工</span>
            <span class="ript">{{detail.weixiugong}}</span>
        </div>
        <div v-if="detail.state==1" class="ipts">
            <span class="ititle">到场时间</span>
            <span class="ript">{{detail.arrivetime}}</span>
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
                <i-icon style="position:relative;top:-4rpx;" type="coordinates_fill" size="26" color="#2d8cf0"
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
        <i-row :class="'buttons'" v-if="peopletype=='gz'">
            <i-col v-if="detail.state=='0'" :span="16">
                <i-button type="primary" @click="jiedan">接 单</i-button>
            </i-col>
            <i-col v-else="detail.state=='1'" :span="16">
                <i-button type="primary" @click="finish">确认完成</i-button>
            </i-col>
            <i-col :span="8">
                <i-button type="warning" @click="fankui">反 馈</i-button>
            </i-col>
        </i-row>
        <view class="paidan" v-if="paidanShow">
            <view class="paidancard">
                <img src="/static/images/close.png" class="cardclose" @click="paidanToggle">
                <span class="cardtitle">派单</span>
                <span class="cardline">
                    <span class="linetitel">*维修人员</span>
                    <span class="linedetail" @click.stop="selectPeople">{{peoples}} > </span>
                </span>
                <span class="cardline" style="margin-bottom:75rpx;">
                    <span class="linetitel">*到场时间</span>
                    <span class="linedetail" @click.stop.click="dateTimePick">{{dateSelected}} > </span>
                </span>
                <view class="sure">
                    <i-button type="primary">确认派单</i-button>
                </view>
            </view>
            <van-popup :show="datepickershow" position="bottom">
                <van-datetime-picker :type="'datetime'" :data-value="currentDate" :value="peoplecurrent"
                                     v-model="currentDate" :min-date="minDate" @confirm="confirm" @cancel="cancel"/>
            </van-popup>
            <van-popup :show="peoplepickershow" position="bottom">
                <view class="check">
                    <span class="ckitem ckitem1" @click="peosltcancel">取消</span>
                    <span class="ckitem ckitem2" @click="peosltsure">确定</span>
                </view>
                <scroll-view scroll-y="true" :style="{ height: '460rpx'}">
                    <i-panel>
                        <i-checkbox-group :current="peoplecurrent" :data-set="peoplecurrent"
                                          @change="handleFruitChange">
                            <i-checkbox v-for=" (item,index) in houxuanren" :position="'right'" :key="index"
                                        :value="item.name">
                            </i-checkbox>
                        </i-checkbox-group>
                    </i-panel>
                </scroll-view>
            </van-popup>
        </view>
        <view class="fankui" v-if="fankuiShow" @touchmove.stop="">
            <view class="fankuicard">
                <img src="/static/images/close.png" class="cardclose" @click="fankuiToggle">
                <span class="cardtitle">反馈</span>
                <span class="fklytxt">*反馈理由</span>
                <view class="section">
                    <view class="reasons">
                        <picker @change="bindPickerChange" :value="index" :range="array">
                            <view class="picker">
                                {{array[index]?array[index]+'√':'请选择理由'}}
                            </view>
                        </picker>
                        <picker @change="bindPickerChange2" :value="index2" :range="array2">
                            <view class="picker2">
                                {{array2[index2]?array2[index2]:'请选择理由'}}
                            </view>
                        </picker>
                    </view>
                </view>
                <span class="fklytxt">备注</span>
                <textarea v-if="fankuiShow" class="fklyDesc" @touchmove.prevent="" v-model="fklyDesc" type="textarea"
                          maxlength="200" autofocus placeholder="  输入备注内容（200字内）"></textarea>
                <span class="fklytxt">录音汇报</span>
                <button class="fkbtns" @longpress="start" @touchmove="handleTouchMove" @touchend="stop">{{luyinwenzi}}
                </button>
                <button v-if="playVoiceBtnShow" class="fkbtns voiceplay" @tap='play'>
                    <img src="/static/images/voice.png" class="voice">
                    <span> {{time}}秒</span>
                </button>
                <img v-if="playVoiceBtnShow" src="/static/images/shanchu.png" class="deleteVoice"
                     @click.stop="deleteVoice">
                <view class="fkbtn">
                    <i-button type="primary" @click.stop="fankui">确认反馈</i-button>
                </view>
            </view>
        </view>
        <view class="finish" v-if="finishShow">
            <view class="finishcard">
                <img src="/static/images/close.png" class="cardclose" @click="finishToggle">
                <span class="cardtitle">确认维修完成</span>
                <span class="finishtips">*维修完成，请上传现场反馈图片。</span>
                <div class="uoloadimgs" :data-upid="index" @click="uploadImg" v-for="(item,index) in upload"
                     :key="index">
                    <span>+</span>
                    <span class="tips">上传图片</span>
                    <div v-if="item.show" class="imgbox">
                        <img :src="item.imgurl" :mode="'widthFix'" @click.stop="SelectPreview(index)"
                             class="uploadedimg">
                    </div>
                    <img v-if="item.show" :data-upid="index" src="/static/images/delete.png" @click.stop='deleteImg'
                         class="delete">
                </div>
                <i-button type="primary" @click.stop="finishCertain">确认完成</i-button>
            </view>
        </view>
        <i-toast id="toast"/>
    </div>
</template>
<script>
    import {$Toast} from '../../../static/iview/base/index'

    export default {
        data() {
            return {
                index: 0,
                index2: 0,
                array: ['修补了', '不能修', '不可归我管', '其他'],
                array2: ['修补了detail', '不能修detail', '不可归我管detail', '其他detail'],
                // 时间选择器
                minHour: 10,
                maxHour: 20,
                minDate: new Date().getTime(),
                maxDate: new Date(2019, 10, 1).getTime(),
                currentDate: new Date().getTime(),
                // 时间选择器
                //身份选择
                peopletype: 'gz', // wxg  gz  维修工 和 工区工长
                //身份选择
                //确认完成
                upload: [
                    {imgurl: '', show: false},
                    {imgurl: '', show: false},
                ],
                imgsUrl: [],
                //确认完成
                detail: {
                    state: 1,
                    banzu: '一工队',
                    gongzhang: '张三',
                    gzcell: '13858585654',
                    weixiugong: '李四',
                    wxgcell: '13865656545',
                    arrivetime: '2019-7-11 08:00',
                    jubao: '10086',
                    location: '',
                    fktype: '水表坏',
                    fkbeizhu: "水表坏需要更换，没办法维修。",
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
                paidanShow: false,
                fankuiShow: false,
                finishShow: false,
                datepickershow: false,
                peoplepickershow: false,
                dateSelected: "请选择",
                list: ['a', 'b', 'c'],
                result: ['a', 'b'],
                houxuanren: [{
                    id: 1,
                    name: '香蕉',
                }, {
                    id: 2,
                    name: '苹果'
                }, {
                    id: 3,
                    name: '西瓜'
                }, {
                    id: 4,
                    name: '葡萄1',
                }, {
                    id: 5,
                    name: '葡萄2',
                }, {
                    id: 6,
                    name: '葡萄3',
                }, {
                    id: 7,
                    name: '葡萄4',
                }, {
                    id: 8,
                    name: '葡萄5',
                }],
                peoplecurrent: [],
                position: 'right',
                animal: 'xx',
                checked: false,
                disabled: false,
                fklyDesc: "",
                sendLock: false,
                startY: '',
                luyinwenzi: "长按添加录音",
                playVoiceBtnShow: false,
                timeinterval: '',
                time: 0
            }
        },
        computed: {
            judgement: function () {
                console.log(this.detail.state);
                return (this.detail.state == 2 || this.detail.state == 3) ? true : false
            },
            peoples: function () {
                if (this.peoplecurrent.length > 0) {
                    return this.peoplecurrent
                } else {
                    return "请选择 "
                }
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
                    urls: this.detail.origin.imgsUrl // 需要预览的图片http链接列表
                })
            },
            jiedan() {
                this.paidanShow = true
                console.log("jiedan");
            },
            fankui() {
                console.log("fankui");
                this.fankuiShow = true
            },
            finish() {
                console.log("finsihed");
                this.finishShow = true
            },
            selectPeople: function () {
                console.log("选择人员")
                this.peoplepickershow = true
            },
            dateTimePick: function () {
                console.log("dateTimePick")
                this.datepickershow = true
            },
            confirm(e) {
                console.log(e.mp.detail);
                this.currentDate = e.mp.detail
                var timeSelected = this.formatDate(e.mp.detail)
                this.dateSelected = timeSelected
                console.log(timeSelected)
                this.datepickershow = !this.datepickershow
            },
            cancel: function () {
                console.log("pickerclose");
                this.datepickershow = !this.datepickershow
            },
            formatDate: function (timestamp) {
                var date = new Date(timestamp);
                var Y = date.getFullYear() + '-';
                var M = (date.getMonth() + 1 < 10 ? '0' + (date.getMonth() + 1) : date.getMonth() + 1) + '-';
                var D = (date.getDate() < 10 ? '0' + (date.getDate()) : date.getDate()) + ' ';
                var h = (date.getHours() < 10 ? '0' + (date.getHours()) : date.getHours()) + ':';
                var m = (date.getMinutes() < 10 ? '0' + (date.getMinutes()) : date.getMinutes()) + ' ';
                var s = (date.getSeconds() < 10 ? '0' + (date.getSeconds()) : date.getSeconds());
                return Y + M + D + h + m;
            },
            formatter(type, value) {
                if (type === 'year') {
                    return `${value}年`;
                } else if (type === 'month') {
                    return `${value}月`;
                }
                return value;
            },
            onChange(e) {
                console.log(e);
            },
            handleFruitChange: function (e) {
                var detailvalue = e.mp.detail.value
                const index = this.peoplecurrent.indexOf(detailvalue);
                index === -1 ? this.peoplecurrent.push(detailvalue) : this.peoplecurrent.splice(index, 1);
                this.peoplecurrent = this.peoplecurrent
                console.log(e.mp.currentTarget.dataset.set);
                console.log(this.peoplecurrent);
            },
            peosltcancel() {
                this.peoplepickershow = !this.peoplepickershow
            },
            peosltsure() {
                this.peoplepickershow = !this.peoplepickershow
            },
            paidanToggle() {
                this.paidanShow = !this.paidanShow
            },
            finishToggle() {
                this.finishShow = !this.finishShow
            },
            fankuiToggle() {
                this.fankuiShow = !this.fankuiShow
            },
            bindPickerChange(e) {
                console.log('picker发送选择改变，携带值为', e.mp.detail.value)
                this.index = e.mp.detail.value
            },
            bindPickerChange2(e) {
                console.log('picker发送选择改变，携带值为', e.mp.detail.value)
                this.index2 = e.mp.detail.value
            },
            play: function () {
                const innerAudioContext = wx.createInnerAudioContext()
                //播放声音文件
                innerAudioContext.autoplay = true
                innerAudioContext.src = this.tempFilePath,
                    innerAudioContext.onPlay(() => {
                        console.log('开始播放')
                    })
                innerAudioContext.onError((res) => {
                    console.log(res.errMsg)
                    console.log(res.errCode)
                })
            },
            start: function (e) {
                console.log(e.y)
                this.startY = e.y
                $Toast({
                    content: '录音中，上滑取消',
                    image: '/static/images/record.png',
                    duration: 0,
                });
                //开始录音
                console.log("开始录音")

                const options = {
                    duration: 10000, //指定录音的时长，单位 ms
                    sampleRate: 16000, //采样率
                    numberOfChannels: 1, //录音通道数
                    encodeBitRate: 96000, //编码码率
                    format: 'mp3', //音频格式，有效值 aac/mp3
                    frameSize: 50, //指定帧大小，单位 KB
                }
                this.luyinwenzi = "松开 结束"
                const recorderManager = wx.getRecorderManager()
                //开始录音
                recorderManager.start(options);
                recorderManager.onStart(() => {
                    console.log('recorder start')
                });
                //错误回调
                recorderManager.onError((res) => {
                    console.log(res);
                })
            },
            stop: function () {
                //结束录音
                console.log("结束录音")
                $Toast.hide();
                const recorderManager = wx.getRecorderManager()
                recorderManager.stop();
                recorderManager.onStop((res) => {
                    console.log(res);
                    if (res.duration < 1000) {
                        $Toast({
                            content: '录音时间太短',
                            image: '/static/images/back.png',
                            duration: 3,
                        });
                    } else {
                        this.time = Math.ceil(res.duration / 1000)
                        this.tempFilePath = res.tempFilePath;
                        console.log('停止录音', res.tempFilePath)
                        const {tempFilePath} = res;
                        this.playVoiceBtnShow = true
                    }
                })
            },
            handleTouchMove(e) {
                console.log(e.touches[e.touches.length - 1].clientY - this.startY);
                //touchmove时触发一
                var moveLenght = e.touches[e.touches.length - 1].clientY - this.startY; //移动距离
                if (Math.abs(moveLenght) > 400) {
                    $Toast.hide();
                    $Toast({
                        content: '松开手指取消',
                        image: '/static/images/back.png',
                        duration: 3,
                    });
                    this.sendLock = true; //触发了上滑取消发送，上锁
                    this.stop()
                } else if (Math.abs(moveLenght) > 200) {
                    $Toast.hide();
                    $Toast({
                        content: '松开手指取消',
                        image: '/static/images/back.png',
                        duration: 0,
                    });
                    this.sendLock = false; //上划距离不足，依然可以发送，不上锁
                }
            },
            deleteVoice() {
                this.playVoiceBtnShow = false;
                this.luyinwenzi = "长按添加录音"
            },
            SelectPreview: function (k) {
                var wx = mpvue
                var urls = []
                urls.push(this.imgsUrl[k])
                wx.previewImage({
                    current: urls[0], // 当前显示图片的http链接
                    urls: urls // 需要预览的图片http链接列表
                })
            },
            deleteImg(e) {
                var _this = this
                var indexOfLoad = e.mp.currentTarget.dataset.upid
                _this.upload[indexOfLoad].show = false
                _this.upload[indexOfLoad].imgurl = ''
                _this.imgsUrl[indexOfLoad] = ''
            },
            uploadImg(e) {
                var _this = this
                var indexOfLoad = e.mp.currentTarget.dataset.upid
                let wx = mpvue
                console.log("uploadImg")
                let i = 0; // 多图上传时使用到的index
                let that = this;
                let max = that.maxImg; //最大选择数
                let upLength; //图片数组长度
                let imgFilePaths; //图片的本地临时文件路径列表
                wx.chooseImage({
                    count: max || 1, //一次最多可以选择的图片张数
                    sizeType: ['original', 'compressed'], // 可以指定是原图还是压缩图，默认二者都有
                    sourceType: ['album', 'camera'], // 可以指定来源是相册还是相机，默认二者都有
                    success: function (res) {
                        // tempFilePath可以作为img标签的src属性显示图片
                        const tempFilePaths = res.tempFilePaths
                        // console.log(tempFilePaths);
                        _this.imgsUrl[indexOfLoad] = tempFilePaths[0]
                        _this.upload[indexOfLoad].imgurl = tempFilePaths
                        _this.upload[indexOfLoad].show = true
                        /**
                         * 上传完成后把文件上传到服务器
                         */
                        _this.fileUpload(tempFilePaths)
                        // wx.showLoading({
                        //     title: '上传中...',
                        // })
                        // _this.fileUpload(imgFilePaths, i, upLength);         //上传操作
                    },
                    fail: function () {
                        console.log('fail');
                    },
                    complete: function () {
                        console.log('commplete');
                    }
                })
            },
            fileUpload: function (tempFilePaths) {
                console.log(tempFilePaths);
                var wx = mpvue
                var that = this;
                wx.showLoading({
                    title: '上传中...',
                    duration: 2
                })
                wx.getFileSystemManager().readFile({
                    filePath: tempFilePaths[0], //选择图片返回的相对路径
                    encoding: 'base64', //编码格式
                    success: res => { //成功的回调
                        console.log(res);
                    }
                })
                /*const uploadTask = wx.uploadFile({
                    // .....
                })
                uploadTask.onProgressUpdate((res) => {
                    console.log('上传进度', res.progress)
                    console.log('已经上传的数据长度', res.totalBytesSent)
                    console.log('预期需要上传的数据总长度', res.totalBytesExpectedToSend)
                })*/
                /*  var _this = this
                  let wx = mpvue
                  wx.uploadFile({
                      url: "",//url地址， //app.ai_api.File.file
                      filePath: tempFilePath,  //文件路径  这里是mp3文件
                      name: 'file',  //随意
                      header: {
                          'Content-Type': 'multipart/form-data',
                          'Authorization': wx.getStorageSync("access_token"),  //如果需要token的话要传
                      },
                      formData: {
                          method: 'POST'   //请求方式
                      },
                      success(res){
                          var data = JSON.parse(res.data)  // 坑2：与wx.request不同的是，upload返回的是字符串格式，需要字符串对象化
                          if (data.code == 200) {
                              that.fileTrans(data.data.id); //执行接口函数 语音文件转文字
                          } else {
                              console.log('上传失败')
                              wx.showToast({
                                  title: res.message,
                                  icon: 'none'
                              })
                          }
                      }
                  })*/
            },
            finishCertain() {
                console.log("确认完成");
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
        width: 740 rpx;
        min-height: 76 rpx;
        line-height: 76 rpx;
        font-size: 28 rpx;
        display: block;
        margin: 0 auto;
        color: #3f4147;
        border-bottom: 1 rpx solid rgba(222, 222, 222, 0.67);

    }

    .ititle {
        display: block;
        float: left;
        padding-left: 10 rpx;
    }

    .loca {
        float: right;
    }

    .imgs {
        float: none;
    }

    .ript {
        display: block;
        height: 70 rpx;
        line-height: 70 rpx;
        float: right;
        text-align: right;
        padding-right: 10 rpx;
        /*background: #ededed;*/
        width: 525 rpx;

    }

    .riptcontent {
        display: block;
        height: 56 rpx;
        line-height: 56 rpx;
        float: right;
        text-align: right;
        padding-right: 10 rpx;
        width: auto;
        border: 1 rpx solid rgba(63, 65, 71, 0.31);
        padding: 0 10 rpx;
        border-radius: 10 rpx;
        vertical-align: middle;
        margin-top: 10 rpx;

    }

    .fkreason {
        float: right;
        border: 1 rpx solid #1c2438;
        padding: 0 10 rpx;
        height: 50 rpx;
        line-height: 50 rpx;
        margin-top: 18 rpx;
        border-radius: 10 rpx;

    }

    .phone {
        color: #2d8cf0;
    }

    .active .gou {
        display: inline;
    }

    .detailbox {
        width: 100%;
        padding: 20 rpx;
        box-sizing: border-box;
        -webkit-box-sizing: border-box;

    }

    .xqtxt {
        font-weight: bold;
    }

    .detail {
        width: 100%;
        min-height: 600 rpx;
        border: 1 rpx solid rgba(128, 128, 128, 0.37);
        margin-top: 10 rpx;
        padding: 10 rpx;
        box-sizing: border-box;
        box-shadow: 0 0 10 rpx rgba(92, 92, 92, 0.36);

    }

    .bxlist {
        width: 100%;
        display: block;
        min-height: 50 rpx;
        line-height: 50 rpx;
        font-size: 28 rpx;
        text-align: justify;
        padding-left: 10 rpx;
        box-sizing: border-box;
    }

    .spans,
    .neirong {
        display: inline-block;
    }

    .neirong {
        border: 1 rpx solid #b2b8c5;
        border-radius: 10 rpx;
        margin: 0 10 rpx 10 rpx 10 rpx;
        box-sizing: border-box;
        padding: 0 10 rpx;
        font-size: 24 rpx;

    }

    .inspan {
        margin-right: 20 rpx;
    }

    .imgbox {
        display: inline-block;
        width: 200 rpx;
        height: 200 rpx;
        background: #fff;
        margin: 0 20 rpx 0 0;
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
        margin-bottom: 150 rpx;
    }

    .addressworker,
    .fkbeizhu {
        display: block;
        float: none;
        background: #f7f4f4;
        width: 91%;
        margin: 0 auto;
        margin-bottom: 21 rpx;
        line-height: 40 rpx;
        padding: 10 rpx;
        text-align: justify;

    }

    .paidan,
    .fankui,
    .finish {
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
        min-height: 522 rpx;
        background: white;
        border-radius: 20 rpx;
        -webkit-border-radius: 20 rpx;
        position: absolute;
        left: 50%;
        top: 50%;
        transform: translate(-50%, -50%);
        -webkit-transform: translate(-50%, -50%);
    }

    .fankuicard {
        width: 75%;
        height: 800 rpx;
        background: white;
        border-radius: 20 rpx;
        -webkit-border-radius: 20 rpx;
        position: absolute;
        left: 50%;
        top: 50%;
        transform: translate(-50%, -50%);
        -webkit-transform: translate(-50%, -50%);
        padding: 0 30 rpx;
    }

    .finishcard {
        width: 75%;
        height: 600 rpx;
        background: white;
        border-radius: 20 rpx;
        -webkit-border-radius: 20 rpx;
        position: absolute;
        left: 50%;
        top: 50%;
        transform: translate(-50%, -50%);
        -webkit-transform: translate(-50%, -50%);
        padding: 0 30 rpx;
    }

    .cardtitle {
        width: 100%;
        margin: 0 auto;
        text-align: center;
        display: block;
        margin-top: 29 rpx;
        margin-bottom: 37 rpx;

    }

    .cardline {
        display: block;
        margin: 0 auto;
        width: 75%;
        border-bottom: 1 rpx solid rgba(128, 128, 128, 0.5);
        overflow: hidden;
        height: 75 rpx;
        line-height: 75 rpx;
        font-size: 24 rpx;
        margin-bottom: 18 rpx;
    }

    .linetitel {
        float: left;
    }

    .linedetail {
        text-align: right;
        float: right;
        width: 310 rpx;
        /*background:red;*/
        overflow: hidden;

    }

    .sure {
        margin-top: 40 rpx;
        width: 84%;
        margin: 0 auto;

    }

    .check {
        width: 100%;
        height: 88 rpx;
        display: block;
        background: white;
        color: #2d8cf0;
    }

    .ckitem {
        height: 100%;
        width: 50%;
        display: block;
        padding: 0 20 rpx;
        box-sizing: border-box;
        line-height: 88 rpx;

    }

    .ckitem1 {
        float: left;
        text-align: left;
    }

    .ckitem2 {
        float: right;
        text-align: right;
    }

    .picker {
        width: 190 rpx;
        height: 50 rpx;
        line-height: 50 rpx;
        text-align: center;
        font-size: 28 rpx;
        border: 1 rpx solid #0064fa;
        color: #0064fa;
        /*margin-left   : 30rpx;*/
        border-radius: 10 rpx;
        display: inline-block;
        float: left;

    }

    .picker2 {
        width: 325 rpx;
        height: 50 rpx;
        line-height: 50 rpx;
        text-align: center;
        font-size: 28 rpx;
        border: 1 rpx solid #a7a7a7;
        color: #a7a7a7;
        margin-left: 30 rpx;
        border-radius: 10 rpx;
        display: inline-block;
        float: right;

    }

    .reasons {
        display: block;
        width: 100%;
        height: 50 rpx;
    }

    .fklytxt {
        margin: 25 rpx 0;
        display: block;
        font-size: 27 rpx;

    }

    .fklyDesc {
        width: 100%;
        height: 178 rpx;
        background: #f6f6f6;
        border: 1 rpx solid #e1e1e1;
        border-radius: 10 rpx;
        -webkit-border-radius: 10 rpx;
        font-size: 25 rpx;
        line-height: 35 rpx;
        text-align: justify;
        overflow: hidden;
        display: block;

    }

    .fkbtns {
        width: 258 rpx;
        height: 60 rpx;
        float: left;
        line-height: 62 rpx;
        display: block;
        font-size: 25 rpx;
        position: absolute;
        left: 28 rpx;
        top: 560 rpx;

    }

    .voiceplay {
        background: #a9e97b;

    }

    .voice {
        width: 50 rpx;
        height: 50 rpx;
        margin-top: 5 rpx;
        margin-left: 2 rpx;
        float: left;

    }

    .deleteVoice {
        position: absolute;
        width: 82 rpx;
        height: 47 rpx;
        left: 294 rpx;
        top: 569 rpx;

    }

    .cardclose {
        width: 45 rpx;
        height: 45 rpx;
        right: 20 rpx;
        top: 18 rpx;
        position: absolute;
    }

    .fkbtn {
        width: 100%;
        display: block;
        margin-top: 138 rpx;
    }

    .finishtips {
        width: 100%;
        text-align: left;
        font-size: 27 rpx;
    }

    .uoloadimgs {
        display: inline-block;
        width: 150 rpx;
        text-align: center;
        line-height: 107 rpx;
        font-size: 78 rpx;
        height: 150 rpx;
        border: 2 rpx solid #cbcbcb;
        background: #f3f3f3;
        margin: 10 rpx 18 rpx;
        /*border-radius : 16rpx;*/
        color: #d0d0d0;
        position: relative;
        margin-top: 63 rpx;
        margin-bottom: 56 rpx;

    }

    .tips {
        position: absolute;
        width: 100%;
        text-align: center;
        left: 0;
        top: 57 rpx;
        font-size: 20 rpx;
        color: #d0d0d0;

    }

    .finishcard .imgbox {
        width: 100%;
        height: 100%;
        position: absolute;
        left: 0;
        top: 0;
        overflow: hidden;
    }

    .uploadedimg {
        position: absolute;
        left: 0;
        top: 50%;
        width: 100%;
        /*height   : 100%;*/
        z-index: 1;
        transform: translateY(-50%);
        -webkit-transform: translateY(-50%);
        background: white;

    }

    .delete {
        position: absolute;
        right: -18 rpx;
        top: -18 rpx;
        width: 36 rpx;
        height: 36 rpx;
        z-index: 2;

    }

</style>
