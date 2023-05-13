<template>
	<!-- // +----------------------------------------------------------------------
// | Copyright (c) 2020~2022 https://www.sdwanyue.com All rights reserved.
// +----------------------------------------------------------------------
// | 万岳科技相关开源系统代码并不是自由软件，未经授权许可不能去掉wanyue【万岳科技】相关版权并商用
// | 万岳科技相关开源系统，需标注"代码来源于万岳科技开源项目"后即可免费自用运营，前端运营时展示的内容不得使用万岳科技相关信息
// +----------------------------------------------------------------------
// | Author: 万岳科技开源官方 < wanyuekj2020@163.com >
// +---------------------------------------------------------------------- -->
	<view>
		
	<!-- #ifndef H5 -->
	<uni-nav-bar :border="false" @clickLeft="back" left-icon="back" title="万岳教育">
		
	</uni-nav-bar>
	<!-- #endif -->
		
	<web-view :src="url"></web-view>
		
	</view>
</template>

<script>
	import uniNavBar from '@/components/uni-ui/uni-nav-bar/uni-nav-bar.vue';
	
	export default{
		components: {
			uniNavBar,
		},
		data(){
			return{
				url:''	
			}
		},
		onLoad(option) {
			uni.setNavigationBarTitle({
				title:option.title
			})
			this.url = JSON.parse(decodeURIComponent(option.url));
		},
		methods:{
			back(){
				uni.navigateBack({
					delta: 1
				});
			}
		}
	}
</script>

<style>
	
</style>
