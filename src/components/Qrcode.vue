<template>
  <!-- 海报html元素 -->
  <div id="posterHtml" :style="{backgroundImage: 'url('+posterHtmlBg+')'}">
    <div>{{posterContent}}</div>
    <!-- 二维码 -->
    <div class="qrcode" v-if="!posterImg">
      <canvas id="qrcodeImg"></canvas>
    </div>
    <img :src="posterImg" class="poster" />
  </div>
</template>

<script>
import QRCode from "qrcode";
import html2canvas from "html2canvas";
export default {
  name: "Qrcode",
  props: {},
  data() {
    return {
      posterContent: "", // 文案内容
      posterHtmlBg: require("../assets/images/logo.png"), // 背景图
      posterImg: "" // 最终生成的海报图片
    };
  },

  mounted() {
    this.createQrcode(
      "https://blog.csdn.net/weixin_42890953/article/details/82776760"
    );
    this.createPoster();
  },

  methods: {
    /**
     * @desc 生成二维码
     */
    createQrcode(text) {
      const canvas = document.getElementById("qrcodeImg");
      QRCode.toCanvas(
        canvas,
        text,
        {
          errorCorrectionLevel: "H",
          margin: 1,
          width: 20,
          color: {
            dark: "#010599FF",
            light: "#FFBF60FF"
          }
        },
        (err, url) => {
          if (err) console.error(err);
          // HTMLCanvasElement
          console.log("QRCode success!" + url);
        }
      );
      QRCode.toDataURL("I am a pony!", function(err, url) {
        // base64
        console.log(url);
      });
      QRCode.toString("I am a pony!", { type: "terminal" }, function(err, url) {
        // svg
        console.log(url);
      });
    },
    /**
     * @desc 生成海报
     */
    createPoster() {
      const vm = this;
      const domObj = document.getElementById("posterHtml");
      html2canvas(domObj, {
        useCORS: true,
        allowTaint: false,
        logging: false,
        letterRendering: true,
        onclone(doc) {
          let e = doc.querySelector("#posterHtml");
          e.style.display = "block";
        }
      }).then(function(canvas) {
        // 在微信里,可长按保存或转发
        vm.posterHtmlBg = "";
        vm.posterImg = canvas.toDataURL("image/png");
      });
    }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
#posterHtml {
  height: 100vh;
  background-size: 100% 100%;
  position: relative;
}
.qrcode {
  position: absolute;
  bottom: 0;
  right: 0;
}
.poster {
  width: 100vh;
  height: 100vh;
}
</style>
