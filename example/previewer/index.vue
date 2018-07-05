<template>
  <div class="home">
    <image-previewer :images="list"></image-previewer>
  </div>
</template>

<script>

  import imagePreviewer from './children/image-previewer'

  export default {
	name: 'home',
	components: {imagePreviewer},
	data() {
	  return {
		list: [
		  '/static/1.png',
		  '/static/2.jpg',
		  '/static/3.jpg',
		  '/static/4.jpg',
		  '/static/5.jpg',
		  '/static/6.jpg',
		  '/static/7.jpg',
		],
	  }
	},
	methods: {},
	computed: {}
  }
</script>

<style scoped lang="less" rel="stylesheet/less">

  .home {
    padding: 20px;

  }
</style>
