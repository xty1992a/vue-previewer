<template>
  <div class="preview-container" @mousewheel.prevent="wheelHandler" @DOMMouseScroll.prevent="wheelHandler">
    <div class="previewer" :class="[size]" ref="wrap"
         @mouseenter="enter = true"
         @mousemove="setOffset($event)"
         @mouseleave="enter = false">
      <img :src="src" alt="">
      <div class="cell" ref="cell" v-show="enter" :style="cellStyle"></div>
    </div>
    <div class="shower" :style="showStyle" v-show="enter">
    </div>
  </div>
</template>

<script>
  const ranger = (val, min, max) => Math.min(Math.max(min, val), max)
  export default {
	name: 'previewer',
	components: {},
	props: {
	  src: String,
	  size: {
		type: String,
		default: 'wrap'
	  },
	  // 黄色窗口与图片的比例，同样也是预览窗口与背景图的比例
	  ratio: {
		type: Number,
		default: 0.5
	  },
	  // 预览窗口使用该值缩放
	  scale: {
		type: Number,
		default: 1
	  },
	  ratioRanger: {
		type: String,
		default: '1,9'
	  }
	},
	data() {
	  return {
		enter: false,
		point: {
		  x: 0,
		  y: 0
		},
		width: 0,
		height: 0,
		localRatio: 0.5
	  }
	},
	mounted() {
	  this.localRatio = this.ratio
	  setTimeout(() => {
		this.init()
	  }, 20)
	},
	methods: {
	  init() {
		let wrap = this.$refs.wrap
		this.width = wrap.clientWidth
		this.height = wrap.clientHeight
	  },
	  setOffset({offsetX: x, offsetY: y}) {
		this.point = {x, y}
	  },
	  wheelHandler(e) {
		// firefox使用detail:下3上-3,其他浏览器使用wheelDelta:下-120上120//下滚
		let delta = -e.wheelDelta || e.detail
		let [min, max] = this.ratioRanger.split(',')
		min = Math.max(0.1, min / 10)
		max = Math.min(1, max / 10)
		let ratio = this.localRatio
		if (delta > 0) {
		  ratio -= 0.1
		}
		if (delta < 0) {
		  ratio += 0.1
		}
		this.localRatio = ranger(ratio, min, max) //Math.min(Math.max(0.1, ratio), 0.9)
	  }
	},
	computed: {
	  // 黄色窗口的一半宽高
	  halfCell() {
		let width = this.width * this.localRatio * 0.5
		let height = this.height * this.localRatio * 0.5
		return {width, height}
	  },
	  // 黄色窗口相对于图片的偏移值（0<offset<两者之差）
	  offset() {
		let {x, y} = this.point
		let {height, width} = this.halfCell
		// 使鼠标居于黄色窗口正中
		x -= width
		y -= height
		x = ranger(x, 0, this.width * (1 - this.localRatio))
		y = ranger(y, 0, this.height * (1 - this.localRatio))
		return {x, y}
	  },
	  // region 动态样式
	  cellStyle() {
		let {x, y} = this.offset
		return {
		  width: this.localRatio * this.width + 'px',
		  height: this.localRatio * this.height + 'px',
		  transform: `translate3d(${x}px,${y}px,0)`
		}
	  },
	  showStyle() {
		let {x, y} = this.offset
		let width = this.width / this.localRatio
		let height = this.height / this.localRatio
		let oX = x / this.width * width
		let oY = y / this.height * height
		return {
		  width: this.width + 'px',
		  height: this.height + 'px',
		  backgroundImage: `url(${this.src})`,
		  backgroundPosition: `-${oX}px -${oY}px`,
		  backgroundSize: `${width}px ${height }px`,
		  transform: `scale(${this.scale})`
		}
	  }
	  // endregion
	},
  }
</script>

<style scoped lang="less" type="text/less" rel="stylesheet/less">

  .preview-container {
    width: 100%;
    height: 100%;
    position: relative;
  }

  .previewer {
    position: relative;
    cursor: move;
    overflow: hidden;
    // 避免cell出现时，阻止事件，把after盖在它的上方
    &:after {
      position: absolute;
      top: 0;
      left: 0;
      right: 0;
      bottom: 0;
      content: '';
    }
    .cell {
      position: absolute;
      left: 0;
      top: 0;
      background-color: rgba(255, 255, 0, .4);
    }
  }

  .shower {
    z-index: 10;
    box-shadow: 0 0 1px #ccc;
    transform-origin: 0 0;
    background-repeat: no-repeat;
    position: absolute;
    overflow: hidden;
    right: -102%;
    top: 0;
  }

  .wrap {
    height: 100%;
    width: 100%;
    img {
      height: 100%;
      width: 100%;
    }
  }
</style>
