<template>
  <div class="image-previewer">
    <div class="preview-wrap">
      <previewer :src="path" :ratio="0.5" :scale="1.8" ratio-ranger="1,10"></previewer>
    </div>
    <div class="image-list">
      <div class="list-btn" :class="{'disable':pageIndex===0}" @click="setIndex(false)"> <</div>
      <div class="center">
        <ul class="list" :style="listStyle">
          <li class="item"
              @mouseover="currentIndex=index"
              v-for="item,index in images"
              :class="currentIndex===index?'current':''">
            <img :src="item" alt="">
          </li>
        </ul>
      </div>
      <div class="list-btn" :class="{'disable':pageIndex===maxIndex}" @click="setIndex(true)"> ></div>
    </div>
  </div>
</template>

<script>
  import Previewer from '@/components/previewer/VuePreviewer.vue'

  export default {
	name: 'image-previewer',
	components: {Previewer},
	props: {
	  images: {
		type: Array,
		required: true
	  }
	},
	data() {
	  return {
		currentIndex: 0,
		pageIndex: 0
	  }
	},
	methods: {
	  setIndex(isNext = true) {
		let index = this.pageIndex
		let offset = 0
		if (isNext) {
		  index++
		} else {
		  index--
		  offset = 2
		}
		this.pageIndex = Math.min(this.maxIndex, Math.max(index, 0))
		this.currentIndex = this.pageIndex * 3 + offset
//		console.log()
	  },
	},
	computed: {
	  path() {
		return this.images[this.currentIndex]
	  },
	  listStyle() {
		let x = this.pageIndex * 330
		return {
		  width: (this.images.length * 110) + 'px',
		  transform: `translateX(-${x}px)`
		}
	  },
	  maxIndex() {
		return Math.ceil(this.images.length / 3) - 1
	  }
	}
  }
</script>

<style scoped lang="less" rel="stylesheet/less">

  .image-previewer {
    .preview-wrap {
      height: 223.3px;
      width: 397px;
      box-shadow: 0 0 1px #000;
    }
    .image-list {
      padding-top: 10px;
      width: 397px;
      display: flex;
      .list-btn {
        line-height: 67.5px;
        font-size: 50px;
        cursor: pointer;
        width: 33.5px;
      }
      .disable {
        color: #f7f7f7;
      }
      .center {
        flex: 1;
        overflow: hidden;
      }
      .list {
        transition: .3s linear;
        overflow: hidden;
        .item {
          float: left;
          border: 1px solid transparent;
          width: 90px;
          height: 67.5px;
          margin: 0 10px;
          img {
            width: 100%;
            height: 100%;
          }
        }
        .current {
          border-color: #ff6400;
        }
      }
    }
  }
</style>
