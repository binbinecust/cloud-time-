<template>
  <div class="layout-default">
    <main>
      <m-nav v-if="!isPC"></m-nav>
      <navigation v-if="show && isPC"></navigation>
      <nuxt />
      <footer-com v-if="show && isPC"></footer-com>
      <m-footer-com v-if="!isPC"></m-footer-com>
      <a v-if="!isPC" class="phone" href="tel:18500945333">
        <div class="shake">
          <span class="iconfont">&#xe613;</span>
        </div>
      </a>
    </main>
  </div>
</template>
<script>
import navigation from '@/components/navigation'
import MNav from '@/components/m-navigation/index.vue'
import footerCom from '@/components/footer.vue'
import MFooterCom from '@/components/m-footer/index.vue'

export default {
  components: {
    navigation,
    MNav,
    footerCom,
    MFooterCom
  },
  data() {
    return {
      show: false,
      isPC: this.$store.state.isPC
    }
  },
  mounted() {
    let deviceWidth = document.documentElement.clientWidth
    if (deviceWidth > 640) deviceWidth = 640
    document.documentElement.style.fontSize = deviceWidth / 7.5 + 'px'
    this.show = !/(login|config)/.test(this.$route.fullPath)
    this.initShare()
  },
  methods: {
    initShare() {
      const LoadScript = function(url, callback) {
        let script = document.createElement('script')
        script.type = 'text/javascript'
        const cb = function() {
          callback && callback()
          script.onload = script.onreadystatechange = null
          if (script.parentNode) {
            script.parentNode.removeChild(script)
          }
          script = null
        }
        if (script.readyState) {
          // IE
          script.onreadystatechange = function() {
            if (/loaded|complete/.test(script.readyState)) {
              cb()
            }
          }
        } else {
          script.onload = function() {
            cb()
          }
        }
        script.src = url
        document.body.appendChild(script)
      }

      const WxShare = {
        init: function(
          shareParam = {
            title: '',
            desc: '',
            pic: '',
            url: ''
          }
        ) {
          // loadJs
          LoadScript('//res.wx.qq.com/open/js/jweixin-1.0.0.js', function() {
            const wxShare = {
              init: function(opt) {
                const m = this
                const apiList = [
                  'onMenuShareTimeline',
                  'onMenuShareAppMessage',
                  'onMenuShareQQ',
                  'onMenuShareWeibo',
                  'onMenuShareQZone'
                ]
                const d = {
                  appId: 'gh_53042d1729ae',
                  timestamp: +new Date(),
                  nonceStr: '31231231',
                  signature: 34924234
                }
                wx.config({
                  debug: false, // true?????????????????????????????????false
                  appId: d.appId,
                  timestamp: d.timestamp,
                  nonceStr: d.nonceStr,
                  signature: d.signature,
                  jsApiList: apiList // ???????????????JS????????????
                })
                const wxData = {
                  title: opt.title,
                  desc: opt.desc,
                  link: opt.url,
                  imgUrl: opt.pic,
                  type: 'link',
                  dataUrl: '',
                  success: function() {
                    if (opt.success) {
                      opt.success.call(m, opt)
                    }
                  },
                  cancel: function() {}
                }
                wx.ready(function() {
                  wx.onMenuShareTimeline(wxData) // ??????????????????
                  wx.onMenuShareAppMessage(wxData) // ???????????????
                  wx.onMenuShareQQ(wxData) // ?????????QQ
                  wx.onMenuShareWeibo(wxData) // ?????????????????????
                  wx.onMenuShareQZone(wxData) // ?????????QQ??????
                })
                wx.error(function(res) {
                  // console.log(res);
                })
              }
            }
            const isWeixin = /micromessenger/gi.test(window.navigator.userAgent) // ????????????????????????
            let s = {}

            s = {
              title: shareParam.title || document.title,
              desc:
                shareParam.desc ||
                document
                  .querySelector('meta[name=description]')
                  .getAttribute('content'),
              pic:
                shareParam.pic ||
                'https://t.focus-res.cn/front-end/image/focuslive-app-icon.png',
              url: shareParam.url || window.location.href.split('#')[0]
            }

            if (isWeixin) {
              wxShare.init(s)
            }
          })
        }
      }

      WxShare.init({
        title: '???????????????????????????',
        desc: '?????????????????????????????????????????????',
        pic:
          'https://t-img.51f.com/sh120x120sh/xf/xw/81f9d500-d71c-4d3e-97fe-077c0e140464.JPEG',
        url: window.location.href.split('#')[0]
      })
    }
  }
}
</script>

<style lang="scss">
@import '@/assets/style/global.scss';
html {
  font-family: 'Source Sans Pro', -apple-system, BlinkMacSystemFont, 'Segoe UI',
    Roboto, 'Helvetica Neue', Arial, sans-serif;
  font-size: 16px;
  font-display: swap;
  word-spacing: 1px;
  -ms-text-size-adjust: 100%;
  -webkit-text-size-adjust: 100%;
  -moz-osx-font-smoothing: grayscale;
  -webkit-font-smoothing: antialiased;
  box-sizing: border-box;
  text-align: center;
  scroll-behavior: smooth;
}
body {
  // margin-top: 20px;
  cursor: url('https://bibidaodao.cn/assets/album/cat.ico'), auto;
  // background: linear-gradient(#e0eafc, #cfdef3);
}

main {
  width: 100%;
}
// .layout-default {
//   margin-top: 20px;
// }

/* *,
*:before,
*:after {
  box-sizing: border-box;
  margin: 0;
}

.button--green {
  display: inline-block;
  border-radius: 4px;
  border: 1px solid #3b8070;
  color: #3b8070;
  text-decoration: none;
  padding: 10px 30px;
}

.button--green:hover {
  color: #fff;
  background-color: #3b8070;
}

.button--grey {
  display: inline-block;
  border-radius: 4px;
  border: 1px solid #35495e;
  color: #35495e;
  text-decoration: none;
  padding: 10px 30px;
  margin-left: 15px;
}

.button--grey:hover {
  color: #fff;
  background-color: #35495e;
} */

.phone {
  position: fixed;
  bottom: 100px;
  right: 20px;
  z-index: 99999;
  background: linear-gradient(#ff4b52, #f7ad9b);
  display: inline-block;
  width: 100px;
  height: 100px;
  border-radius: 20px;
  text-align: center;
  line-height: 100px;

  .iconfont {
    font-size: 50px;
    color: white;
  }

  .shake {
    animation: shake 4s linear infinite;
  }
}

@keyframes shake {
  0%,
  80% {
    transform: rotate(0);
  }

  85% {
    transform: rotate(30deg);
  }

  90% {
    transform: rotate(-30deg);
  }

  95% {
    transform: rotate(30deg);
  }

  100% {
    transform: rotate(0deg);
  }
}
</style>
