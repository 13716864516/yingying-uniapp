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

		<view style="background-color: #0d5e4f;" class="flex flex-column pb-2">
				<image style="" @click="backPre" class="t-back-img" src="../../static/images/back1.png" mode=""></image>
			<text style="" class="mystyle">我的课程 （{{yearmoth}}）</text>
		</view>

		<view style="background-color: #0d5e4f;">
			<view style="background-color: #0d5e4f;" class="button-sp-area">
				<button  class="mini-btn" type="primary" size="mini" @click="mycourse(-1)">上一周</button>
				<button class="mini-btn mini-btn-right" type="primary" size="mini" @click="mycourse(1)">下一周</button>
			</view>
		</view>
		<view>
			<timetable :timetables="timetables" :timetableskey="timetableskey" :timetableType="timetableType" :weeks="weeks" :weekdate="weekdate" :weeknum="weeknum"></timetable>
		</view>
		
	</view>
</template>

<script>
    import Timetable from '@/components/lpx-timetable/lpx-timetable';
	const app = getApp();
	
	export default {
		components:{
			Timetable,
		},
		data() {
            return {
				weeknum: 0,
				yearmoth: '',
				timetableType: [],
				weeks: [],
				weekdate: [],
                timetables: [],
				timetableskey: [],
            }
		},
		onShow: function() {
			uni.getStorage({
				key: 'userinfo',
				success: function(res) {
					app.globalData.userinfo = res.data;
					this.userInfo = app.globalData.userinfo;
				}
			});
			if(app.globalData.userinfo.token == undefined || app.globalData.userinfo == '') {
				uni.navigateTo({
					url: '../login/login'
				})
				return;
			}
			
			let gData = app.globalData;
			this.weeknum = app.globalData.mycourse.weeknum;
			uni.request({
				url: gData.site_url + 'User.GetBaseInfo',
				method: 'GET',
				data: {
					'uid': gData.userinfo.id,
					'token': gData.userinfo.token,
				},
				success: res => {
					if (parseInt(res.data.data.code) !== 0) {
						return;
					}
					this.userInfo = res.data.data.info[0];
					this.integral = res.data.data.info[0].integral;
					if (res.data.data.info[0].type == '1') {
						//讲师
						this.isTeacherInfo = '1'
					}
				},
			});
			uni.request({
				url: gData.site_url + 'CourseCopy.getYearMonth',
				method: 'GET',
				data: {
					'uid': gData.userinfo.id,
					'token': gData.userinfo.token,
					'weeknum': this.weeknum
				},
				success: res => {
					if (parseInt(res.data.data.code) !== 0) {
						return;
					}
					this.yearmoth = res.data.data.info;
				},
			});
			uni.request({
				url: gData.site_url + 'CourseCopy.getTimeTables',
				method: 'GET',
				data: {
					'uid': gData.userinfo.id,
					'token': gData.userinfo.token,
					'weeknum': this.weeknum
				},
				success: res => {
					if (parseInt(res.data.data.code) !== 0) {
						return;
					}
					this.timetables = res.data.data.info.value;
					this.timetableskey = res.data.data.info.key;
				},
			});
			uni.request({
				url: gData.site_url + 'CourseCopy.getCourseTime',
				method: 'GET',
				data: {
					'uid': gData.userinfo.id,
					'token': gData.userinfo.token,
					'weeknum': this.weeknum
				},
				success: res => {
					if (parseInt(res.data.data.code) !== 0) {
						return;
					}
					this.timetableType = res.data.data.info;
				},
			});
			uni.request({
				url: gData.site_url + 'CourseCopy.getWeeks',
				method: 'GET',
				data: {
					'uid': gData.userinfo.id,
					'token': gData.userinfo.token,
					'weeknum': this.weeknum
				},
				success: res => {
					if (parseInt(res.data.data.code) !== 0) {
						return;
					}
					this.weeks = res.data.data.info;
				},
			});
			uni.request({
				url: gData.site_url + 'CourseCopy.getWeekDate',
				method: 'GET',
				data: {
					'uid': gData.userinfo.id,
					'token': gData.userinfo.token,
					'weeknum': this.weeknum
				},
				success: res => {
					if (parseInt(res.data.data.code) !== 0) {
						return;
					}
					this.weekdate = res.data.data.info;
					console.log('请求接口时的weeknum：' + this.weeknum)
				},
			});
		},
		methods: {
			backPre(){
				uni.switchTab({
					url: '../msg/msg',
					fail: () => {
						uni.showToast({
							icon: 'none',
							title: '网络错误'
						});
					},
				});
			},
			// 我的课程
			mycourse(addweeknum) {
				this.weeknum = this.weeknum + addweeknum
				console.log('点击后的weeknum：' + this.weeknum)
				app.globalData.mycourse.weeknum = this.weeknum;
				uni.navigateTo({
					url: '../course/mycourse',
					fail: () => {
						uni.showToast({
							icon: 'none',
							title: '网络错误'
						});
					},
					success: () => {
					},
				});
			},
		},
	}
</script>

<style>
    .button-sp-area {
        margin: 0 auto;
        width: 95%;
    }

    .mini-btn-right {
        float: right;
    }

	.t-back-img {
		padding-top: 20rpx;
		width: 17px;
		height: 17px;
		margin-left: 10px;
	}

	.mystyle {
		padding-left: 20rpx; margin:0 auto; font-size: 17px; font-weight: bold; margin-top: 30rpx; margin-bottom: 30rpx; color: #fff;
	}

</style>
