<template>
	<view class="teacher-main">
		
		<!-- #ifndef H5 -->
		<uni-nav-bar :border="false" @clickLeft="back" left-icon="back" title="新闻">
			
		</uni-nav-bar>
		<!-- #endif -->
		
		<view class="liveinfo-wrap news-wrap" >
			<!-- 新闻列表 -->
			<view class="live-list"  v-for="(item, index) in teacher_info"  :key="index">
				<view class="live-list-img-wrap" @click="viewNewsInfo(item.id)">
					<image class="live-list-img" :src="item.avatar" mode="aspectFill"></image>
					<image class="live-list-img-icon" src="../../static/images/v.png" ></image>					
				</view>
				
				<view class="live-list-info">
					
					<view class="live-c-title" @click="viewNewsInfo(item.id)">{{item.user_nickname}}</view>
					<view class="live-teacher-price" @click="doattention(item.id)">
						<view class="view-nums-title" v-if="item.isattent == '0'">+关注</view>
						<view class="view-nums-title-a" v-else >已关注</view>						
					</view>
				</view>
			</view>
		</view>
		
	</view>
</template>

<script>
	import uniNavBar from '@/components/uni-ui/uni-nav-bar/uni-nav-bar.vue';
	const app = getApp();
	
	export default {
		components: {
			uniNavBar
		},
		data() {
			return {
				teacher_info: {},
			}
		},
		onLoad() {
		},
		onShow() {
			this.getData();
		},
		methods: {
			back() {
				uni.navigateBack({
					delta: 1
				});
			},
			getData(){
				let gData = app.globalData;
				uni.request({
					url: gData.site_url + 'Teacher.GetTeachers',
					method: 'GET',
					data: {
						'uid' : gData.userinfo.id,
						'token' : gData.userinfo.token,
					},
					success: res => {
						let data = res.data.data;
						if(data.code == 0 && data.info.length > 0) {
							this.teacher_info = res.data.data.info;
						}
						
					},
					fail: () => {
						uni.showToast({
							icon: 'none',
							title: '网络错误',
						});
						return;
					},
				});
			},			
			viewNewsInfo(nid) {
				if(app.globalData.userinfo == '') {
					uni.navigateTo({
						url: '../login/login',
					});
					return;
				} else {
					uni.navigateTo({
						url: '../teacherinfo/teacherinfo?touid=' + nid,
					});
				}
				
			},
			doattention(nid){
				let gData = app.globalData;
				if(app.globalData.userinfo == '') {
					uni.navigateTo({
						url: '../login/login',
					});
					return;
				} else {
					console.log(gData.userinfo.id);
					uni.request({
						url: gData.site_url + 'User.SetAttent',
						method: 'GET',
						data: {
							'uid' : gData.userinfo.id,
							'token' : gData.userinfo.token,
							'touid' : nid,
						},
						success: res => {
							this.getData();
						},
						fail: () => {
							uni.showToast({
								icon: 'none',
								title: '网络错误',
							});
							return;
						},
					});					
				}				
			}
		}
	}
</script>

<style>
	.teacher-main{
		background-color: #fff;
		padding-top: 30rpx;
	}
	/* 大班课/内容列表公共样式 */
	.live-list{
		width: 719rpx;
		height: 111rpx;
		padding-top: 20rpx;
		border-bottom: 2rpx solid #F7F7F7;
		display: flex;
		margin: 0rpx auto;
	}
	.live-list-img-wrap{
		width: 91rpx;
		height: 91rpx;
		position: relative;
	}
	.live-list-img{
		width: 91rpx;
		height: 91rpx;
		border-radius: 91rpx;
	}
	.live-list-img-icon{
		width: 29rpx;
		height: 29rpx;
		position: absolute;
		right: 0rpx;
		bottom: 0rpx;
	}
	.live-list-info{
		display: flex;
		justify-content: space-between;
		width: 628rpx;
		
	}
	.live-c-title{
		padding-left: 20rpx;
		line-height: 80rpx;
		width: 540rpx;
	}
	.live-teacher-price{
		padding-right: 20rpx;
		line-height: 90rpx;		
		width: 95rpx;
		text-align: right;
		color: #00be54;
	}
	

	
	
</style>
