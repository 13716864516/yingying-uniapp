<template>
		<!-- // +----------------------------------------------------------------------
	// |万岳科技开源系统 [山东万岳信息科技有限公司]
	// +----------------------------------------------------------------------
	// | Copyright (c) 2020~2022 https://www.sdwanyue.com All rights reserved.
	// +----------------------------------------------------------------------
	// | 万岳科技相关开源系统，需标注"代码来源于万岳科技开源项目"后即可免费自用运营，前端运营时展示的内容不得使用万岳科技相关信息
	// +----------------------------------------------------------------------
	// | Author: 万岳科技开源官方 < wanyuekj2020@163.com >
	// +---------------------------------------------------------------------- -->
	<view>
		<uni-list-item title="账号与安全" @click="open('user-password')"></uni-list-item>
		<uni-list-item title="绑定邮箱" @click="open('user-email')"></uni-list-item>
		<uni-list-item title="资料编辑" @click="open('user-userinfo')"></uni-list-item>
		<uni-list-item title="清除缓存" @click="clear">
			<view slot="right">{{currentSize|format}}</view>
		</uni-list-item>
		<uni-list-item title="意见反馈" @click="open('user-feedback')"></uni-list-item>
		<uni-list-item title="关于社区" @click="open('about')"></uni-list-item>
		
		<view class="py-2 px-2">
			<button class="bg-main text-white" style="border-radius: 50rpx; border: 0;" 
			type="primary">退出登录</button>
			
		</view>
		
	</view>
</template>

<script>
	import uniListItem from '@/components/uni-ui/uni-list-item/uni-list-item.vue';
	export default {
		components:{
			uniListItem
		},
		data() {
			return {
				currentSize:0
			}
		},
		onLoad() {
			this.getStorageInfo();
		},
		filters:{
			format(value){
				return value > 1024 ? (value/1024).toFixed(2) + 'MB' : value.toFixed(2) + 'KB';
			}
		},
		methods: {
			getStorageInfo(){
				let res = uni.getStorageInfoSync();
				this.currentSize = res.currentSize;
			},
			open(path){
				uni.navigateTo({
					//用反引号
					url: `../${path}/${path}`,
				});
			},
			// 清除缓存
			clear(){
				uni.showModal({
					title: '提示',
					content: '是否要清除所有缓存?',
					cancelText: '不清除',
					confirmText: '清除',
					success: res => {
						if(res.confirm){
							uni.clearStorageSync();
							this.getStorageInfo();
							uni.showToast({
								title:"清除成功"
							})
						}
						
					},
					fail: () => {},
					complete: () => {}
				});
			}
		}
	}
</script>

<style>

</style>
