<template>
  <!-- // 外框，固定宽度 -->
  <div class="wrap">
    <!-- 内部滚动框 -->
    <div id="box"
         v-if="list.length">
      <!-- 展示的文字 -->
      <div id="marquee"
           v-html="text"></div>
      <!-- 文字副本，为了实现无缝滚动 -->
      <div id="copy"></div>
    </div>
    <!-- 为了获取text实际宽度 -->
    <div id="node"
         v-if="list.length"
         v-html="text"></div>
  </div>
</template>
<script>
export default {
  name: 'Marquee',
  props: {
    list: { // 滚动列表
      type: Array,
      default() {
        return []
      }
    },
    speed: { // 滚动速度，越小越快
      type: Number,
      default: 5
    }
  },
  methods: {
    // 获取文字text 的计算后宽度
    move() {
      const width = document.getElementById('node').getBoundingClientRect().width
      const box = document.getElementById('box')
      const copy = document.getElementById('copy')
      copy.innerHTML = this.text // 文字副本填充
      let distance = 0 // 位移距离
      // 设置位移
      setInterval(() => {
        distance = distance - 1
        // 如果位移超过文字宽度，则回到起点
        if (-distance >= width) {
          distance = 20
        }
        box.style.transform = `translate3d(${distance}px,0,0)`
      }, this.speed)
    },
    _hideNumber(phone) {
      phone = phone + ''
      phone = `${phone.substr(0, 3)}****${phone.substr(7)}`
      return `<span class="tip" style="margin-right:10px;display:inline-block">恭喜${phone}获得永久免佣特权 </span>`
    }
  },
  computed: {
    text() {
      return this.list.map(item => this._hideNumber(item.phoneNum)).join('')
    }
  },
  mounted() {
    this.move()
  }
}
</script>
<style scoped lang="stylus">
@import '~common/stylus/variable'
@import '~common/stylus/mixin'
// 限制外框宽度，隐藏多余的部分
.wrap
  overflow hidden
  color #fec5b7
// 移动框宽度设置
#box
  width 80000%
// 文字一行显示
#box div
  float left
// 设置前后间隔
#marquee
  margin 0 16px 0 0
// 获取宽度的节点，隐藏掉
#node
  position absolute
  z-index -999
  top -999999px
</style>
