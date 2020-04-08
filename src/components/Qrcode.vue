<template>
  <!-- 海报html元素 -->
  <div id="posterHtml" :style="{backgroundImage: 'url('+posterHtmlBg+')'}">
    <div>{{posterContent}}</div>
    <!-- 二维码 -->
    <div class="qrcode">
      <div id="qrcodeImg"></div>
    </div>
    <div class="flex">
      <button @click="createQrcode">生成二维码</button>
      <button @click="createPoster">生成海报</button>
    </div>
  </div>
</template>

<script>
import QRCode from "qrcodejs2";
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
  methods: {
    /**
     * @desc 生成二维码
     */
    createQrcode(text) {
      const qrcodeImgEl = document.getElementById("qrcodeImg");
      qrcodeImgEl.innerHTML = "";
      let qrcode = new QRCode(qrcodeImgEl, {
        width: 256,
        height: 256,
        colorDark: "#000000",
        colorLight: "#ffffff",
        correctLevel: QRCode.CorrectLevel.H
      });
      qrcode.makeCode(text);
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
        vm.posterImg = canvas.toDataURL("image/png");
        vm.posterHtmlBg = canvas.toDataURL("image/png");
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
}
.flex {
  display: flex;
  justify-content: space-between;
  align-items: center;
}
</style>
