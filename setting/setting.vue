<template>
	<!-- // +----------------------------------------------------------------------
// | 万岳科技教育开源系统 [由山东万岳信息科技有限公司提供]
// +----------------------------------------------------------------------
// | 版权所有 © 2020~2022 https://git.sdwanyue.com，保留所有权利
// +----------------------------------------------------------------------
// | 本开源系统为万岳科技提供的教育行业解决方案，旨在帮助教育机构快速搭建高效便捷的教学管理系统
// +----------------------------------------------------------------------
// | 该开源系统可免费学习使用或自用运营，但在展示前端内容时需注明“代码来源于万岳科技开源项目”，并且不得使用万岳科技相关信息！
// +----------------------------------------------------------------------
// | 作者：万岳科技开源官方 wanyuekj2020@163.com
// +---------------------------------------------------------------------- -->
	<view class="">
		
		<!-- #ifndef H5 -->
		<uni-nav-bar @clickLeft="back" left-icon="back" :border="false" title="设置">
		</uni-nav-bar>
		<!-- #endif -->	
		
		<view class="touce-wrap">
			<view @click="touchMe" class="touce-item">
				<text class="touch_txt">联系我们</text>
				<text class="set-num-txt iconfont icon-jinrujiantou"></text>
			</view>
			<view @click="updateversion" class="touce-item">
				<text class="touch_txt">版本更新</text>
				<text class="set-num-txt">1.0.0</text>
			</view>
			<view @click="clearcache" class="touce-item">
				<text class="touch_txt">缓存清除</text>
				<text class="set-num-txt">{{cacheSize}}MB</text>
			</view>
		</view>
		
		<view @click="logout" class="logout-wrap">
			<text>退出账号</text>
		</view>
		
	</view>
</template>

<script>
	import uniNavBar from '@/components/uni-ui/uni-nav-bar/uni-nav-bar.vue';
	
	const app = getApp();
	
	export default {
		components:{
			uniNavBar
		},
		data() {
			return {
				cacheSize: 0
			}
		},
		methods: {
			back() {
				uni.navigateBack({
					delta: 1
				});
			},
			touchMe(){
				uni.navigateTo({
					url: '../lianxi-we/lianxi-we',
					fail: () => {
						uni.showToast({
							icon: 'none',
							title: '网络错误'
						});
					},
				});
			},
			updateversion(){
				uni.showToast({
					icon: 'none',
					title: '已经是最新版本'
				});
			},
			logout(){
				app.globalData.userinfo = '';
				app.globalData.login_jump.page = '';
				app.globalData.login_jump.keyword = '';
				app.globalData.login_jump.data = {};
				uni.reLaunch({
					url: '../index/index',
				});
			},
			// 获取清除的缓存
			getcache(){
				// #ifdef APP-PLUS
				var self = this;
				plus.cache.calculate(function(size) { //size是多少个字节单位是b
				    //做下面相应的处理
				    if (size < 1024) {
				        self.cacheSize = size + 'B';
				    } else if (size / 1024 >= 1 && size / 1024 / 1024 < 1) {
				        self.cacheSize = Math.floor(size / 1024 * 100) / 100 + 'KB';
				    } else if (size / 1024 / 1024 >= 1) {
				        self.cacheSize = Math.floor(size / 1024 / 1024 * 100) / 100 + 'M';
				    }
				});
				// #endif
			},
			// APP端清除缓存
			clearcache() {
			    // #ifdef APP-PLUS
			    var self = this;
			    //使用plus.cache.clear 清除应用中的缓存数据
			    plus.cache.clear();
			    // #endif
				uni.showToast({
				    title: '清除成功!',
				    icon: 'none',
				    success() {
				        self.cacheSize = '0B'
				    }
				});
			},
		}
	}
</script>

<style>
	
	page {
		background-color: #F2F6FC;
	}	
	
	.setting-all-wrap {
		margin: 0 auto;
		border-top: 4rpx solid #E8E8E8;
	}
	
	.touce-wrap {
		/* width: 95%; */
		height: 277rpx;
		margin: 0 auto;
		background-color: #FFFFFF;
	}
	
	.touce-item {
		height: 90rpx;
		line-height: 90rpx;
		padding-left: 21rpx;
		border-bottom: 2rpx solid #F5F5F5;
	}
	
	.touch_txt {
		display: inline-block;
		width: 82%;
		height: 100%;
		color: #000000;
		font-size: 30rpx;
	}
	
	.set-num-txt {
		display: inline-block;
		width: 15%;
		text-align: right;
	}
	
	
	/* 退出账号 */
	.logout-wrap {
		height: 90rpx;
		line-height: 90rpx;
		margin-top: 20rpx;
		text-align: center;
		font-size: 30rpx;
		color: #FF1B20;
		background-color: #FFFFFF;
	}
	
	
</style>
