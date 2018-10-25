<template>
  <div class="bao">
    <div class="topDiv">
      <div class="bDiv" id="bDiv" v-html="info.marketingtop">
      </div>
      <music :musicsrc="musicsrc"></music>
    </div>

    <div class="topImg">
          <img :src="info.fmurl" />
    </div>

    <div class="jiangpin" v-html="info.content">
    </div>
    <ul class="baoming">
        <li class="bmContent"></li>
        <li class="bmContentOne">
            <span class="dian" id="zuo"></span>
            <span class="zhong">已报名&nbsp;<span id="renshu">{{info.counts}}</span>人</span>
            <span class="dian" id="you"></span>
        </li>
        <li class="countDown">活动截止倒计时</li>
        <li class="sTime">
            <p class="shijian day"><span id="t_d" class="time">00</span>天</p>
            <p class="shijian"><span id="t_h" class="time">00</span>时</p>
            <p class="shijian"><span id="t_m" class="time">00</span>分</p>
            <p class="shijian"><span id="t_s" class="time">00</span>秒</p>
        </li>
    </ul>
    <div class="paihang" id="paiHang">
        <div id="scrollBox" v-html="info.marketingtop1">
          <br/>
        </div>
    </div>

    <div class="yonghu1" id="ajaxData" v-html="info.memberdata">

    </div>
    <div id="pay">

    </div>
    <footer class="iphone" id="anzhuo">
        <span class="call"></span>
        <a href="#paiHang"> <span class="bang"></span></a>
        <span class="zhuanqian" @click="zhuanqian">
        <img src="../assets/img/zhuan1.png" class="rotate"/>
      </span>
        <span class="call" @click="showcall"></span>

        <span class="btn" @click="btn">{{info.showText}}</span>
        <a href="#paiHang"><span class="bang"></span></a>
        <!--弹窗分割线-->
        <div class="shadow-lay" v-show="shadowlay" @click="shadowlaybtn"></div>
        <div class="callme" v-show="callme">
            <p class="share">
                <span class="money">咨询热线</span></p>
            <p class="dianpu"><span id="orgName">{{info.dpname}}</span></p>
            <p class="men">门店电话</p>
            <div class="phone">
                <p class="phoneFont">
                    <span id="orgTel">{{info.phone}}</span>
                </p>
                <a href="tel:{{info.phone}}" id="aorgTel" style="text-decoration:none">
                    <img src="../assets/img/callone.png" class="phoneIcon" />
                </a>
            </div>
            <p class="men hui">投诉电话</p>
            <div class="phone">
                <p class="phoneFont hui">13580512595</p>
                <a href="tel:13580512595" id="aorgTel1" style="text-decoration:none">
                    <img src="../assets/img/calltwo.png" class="phoneIcon" />
                </a>
            </div>
        </div>
        <!--电话弹窗-->
        <div class="prompt-div" v-show="promptdiv">
            <p class="share">
                <span class="money">分享赚红包</span>
                <span class="close" @click="close">关闭</span>
            </p>
            <p class="plase">请留下联系方式以便我们及时联系您!</p>
            <!-- <p style="width:100%;height:12rem;margin-top:-2%;"> -->
                <input type="text" class="ipt" placeholder="请输入姓名" id="name" v-model="info.uname">
                <input type=" text" placeholder="请输入电话号码" id="tel" v-model="info.uphone">
                <button class="pay" id="goPay" @click="pay">支付{{info.payprice}}立即参加</button>
            <!-- </p> -->
        </div>
    </footer>
    <div class="hongbao">
        <span class="guan">×</span>
        <p class="kai" onclick="getRed();"></p>
    </div>
    <div class="hongbao2">
        <p class="jine">￥&nbsp;<span class="big" id="redMoney">1</span></p>
        <p class="off">关闭</p>
        <p class="cunru">红包已存入微信零钱账户</p>
    </div>
    <!-- <div id="app1" v-show="app1">
      <div class="hb_box201808" id="hb_list">
        <div class="hb_content201808">
          <div class="hb_icon201808" style="background-image: url({{memberHeardUrl}});">
          </div>
          <p class="hb_name201808">{{myName}}</p>
          <p class="hb_name201808" id="daiyan">我为{{orgName}}代言</p>
          <p class="hb_ts201808">扫一扫下面的二维码即可参加活动，还可获得红包</p>
          <div class="ewm_box201808"><img class="ewm_img201808" src='urlqr?url={{aesEn}}'>
          </div>
          <p class="ts_txt201808">长按识别二维码</p>
        </div>
      </div>
      <div class="mub201808" id="mubu1" v-show="mubu1">
      </div>
      <div class="fx_box201808" id='fx_box' v-show="fx_box">
        <div class="fx_header201808">
          <img src='../assets/img/fx.png' class="fx_img201808" />
        </div>
        <div class="fx_bottom201808">
          <div class="fx_list201808" id=fx>
            <div class="fx_icon201808 fx_img201808" style='background-image: url(../assets/img/zf.png);'>
            </div>
            <span class="fx_txt201808">发送给朋友</span>
          </div>
        </div>
        <div class="fx_list201808" id=hb>
          <div class="fx_icon201808 hb_img" style='background-image: url(../assets/img/erwena.png);'>
          </div>
          <span class="hb_txt201808">二维码海报</span>
        </div>
      </div>
    <div class="fx_bg201808" id="fx_bg" style='background-image: url(../assets/img/fxbg.png);'>
    </div> -->
  </div>
  </div>
</template>

<script>
import wx from "weixin-js-sdk";
import Music from "@/components/Music.vue";

export default {
  components: {
    Music
  },
  async mounted() {
    this.musicsrc = window.music;
    const id = window.infoid;
    const shareid = window.shareid;
    await this.$http
      .get("mmwk/sjlm/getjssdk", {
        params: { url: location.href.split("#")[0] }
      })
      .then(res => {
        const da = res.data.data;
        console.log(da);
        wx.config({
          debug: false, // 开启调试模式,
          appId: da.appId, // 必填，企业号的唯一标识，此处填写企业号corpid
          timestamp: da.timestamp, // 必填，生成签名的时间戳
          nonceStr: da.nonceStr, // 必填，生成签名的随机串
          signature: da.signature, // 必填，签名，见附录1
          jsApiList: [
            "scanQRCode",
            "checkJsApi",
            "onMenuShareTimeline",
            "onMenuShareAppMessage"
          ] // 必填，需要使用的JS接口列表，所有JS接口列表见附录2
        });
        wx.error(function(res) {
          alert("error");
        });
        wx.ready(function() {
          document.addEventListener(
            "WeixinJSBridgeReady",
            function() {
              console.log("WeixinJSBridgeReady");
              document.getElementById("myAudio").play();
            },
            false
          );
        });
      });

    await this.$http
      .get("mmwk/sjlm/getinfo", { params: { id, shareid } })
      .then(res => {
        this.info = res.data.info;
        this.info.showText = this.info.isPay === 1 ? "立即分享" : "立即抢购";
        // console.log(this.info);
        setTimeout(this.getDJS, 0);
        setInterval(this.getDJS, 1000);
        // setInterval(this.autoScroll("scrollBox"), 500);
      });
    // await this.$http({
    //   method: "post",
    //   url: "mmwk/sjlm/memberdata",
    //   data: { docid: id },
    //   responseType: "json"
    // }).then(res => {
    //   //   this.info.memberdata = res.data.data;
    //   console.log(this.info.memberdata);
    //   document.getElementById("ajaxData").append(res.data.data);
    // });
  },
  data: () => ({
    memberHeardUrl: "",
    app1: false,
    fx_box: true,
    mubu1: true,
    callme: false,
    shadowlay: false,
    promptdiv: false,
    musicsrc: "",
    info: {
      marketingtop1: "",
      marketingtop: "",
      memberdata: "",
      showText: "",
      content: "",
      music: "",
      fmurl: "",
      counts: "",
      payprice: "",
      uphone: "",
      uname: "",
      uphone: "",
      dpname: "",
      phone: ""
    }
  }),
  methods: {
    autoScroll(obj) {
      const o = document.getElementById(obj);
      const f = o.find("ul");
      f.animate(
        {
          marginTop: "-=0.6rem"
        },
        function() {
          $(this)
            .css({
              marginTop: "0px"
            })
            .find("li:first")
            .appendTo(this);
        }
      );
    },
    getDJS() {
      var EndTime = this.info.etime1; //  初始化结束日期
      var NowTime = new Date();
      var t = EndTime - NowTime.getTime();
      if (t > 0) {
        var d = Math.floor(t / 1000 / 60 / 60 / 24);
        var h = Math.floor((t / 1000 / 60 / 60) % 24);
        var m = Math.floor((t / 1000 / 60) % 60);
        var s = Math.floor((t / 1000) % 60);
        document.getElementById("t_d").innerHTML = d;
        document.getElementById("t_h").innerHTML = h;
        document.getElementById("t_m").innerHTML = m;
        document.getElementById("t_s").innerHTML = s;
      }
    },
    showcall() {
      this.callme = true;
      this.shadowlay = true;
      // document.getElementsByClassName("shadow-lay").css("display", "block");
      // document.getElementsByClassName("callme").css("display", "block");
    },
    shadowlaybtn() {
      this.callme = false;
      this.shadowlay = false;
      this.promptdiv = false;
    },
    btn() {
      if (this.info.isPay == 0) {
        this.promptdiv = true;
        this.shadowlay = true;
      } else {
        tishi();
        // this.app1 = true;
        // this.fx_box = true;
        // this.mubu1 = true;
      }
    },
    zhuanqian() {
      this.btn();
    },
    close() {
      this.promptdiv = false;
      this.shadowlay = false;
    },
    pay() {
      this.shadowlay = false;
      this.promptdiv = false;
    }
  }
};
</script>
