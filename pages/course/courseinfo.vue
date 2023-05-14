<template>
	<view>
		<view class="top" style="">
			<view>
				<image @click="backmycourse" class="t-back-img" src="../../static/images/back1.png" mode=""></image>
			</view>
			<view class="classinfo" style="">
				<text style="">课程信息</text>
			</view>
		</view>

		<!-- 头部 -->
		<view class="p-3 border-bottom avatar-wrap">

			<!-- 老师名字ID区域	 -->
			<view class="title-wrap">
				<image :src="courseDetail.userinfo.avatar_thumb" style="width: 120rpx; height: 120rpx;" class="rounded-circle title-avatar">
				</image>
				<view class="name-id-wrap">
					<text>{{courseDetail.classname}}（{{courseDetail.userinfo.user_nickname}}）</text>
					<text class="id-wrap">{{courseDetail.courserange}}</text>
					<text class="iconfont icon-jinrujiantou" @click="showTeacherInfo(courseDetail.userinfo.id)"></text>
				</view>
			</view>

			<!-- 关注老师 -->
			<view class="guan-wrap">
				<view class="guan-info-wrap">
					<view class="guanzhu">
						{{courseDetail.classroomname}}&nbsp;|
					</view>
					<text>{{courseDetail.lessons}}课时&nbsp;|&nbsp;{{courseDetail.stunum}}学生</text>
				</view>
				<!-- 讲师主页按钮 -->
				<view @click="showTeacherInfo(userInfo.id)" class="teacher-info-btn">
					暂未完成(学生作品)
				</view>
			</view>
		</view>

		<!-- 公共tab -->

		<template>
			<view class="p-3">

				<view v-for="(item, index) in stuList" :key="item.uid" :ref="'item'+index" @click="mycourse(item.uid)">
					<view class="my-item">
						<image class="userinfo-icon-img" :src="item.avatar_thumb" mode="aspectFill"></image>
						<view class="userinfo-title-txt">{{item.user_nickname}}</view>
						<uni-icons :size="20" class="uni-icon-wrapper" color="#bbb" type="arrowright" />
					</view>暂未完成(签到撤销)
					<view class="itemline"></view>
				</view>

			</view>
		</template>

	</view>
</template>

<script>
	const app = getApp();

	export default {
		components: {
		},
		data() {
			return {
				userInfo: {},
				isTeacherInfo: '',
				integral: 0,
				courseDetail: {userinfo: {user_nickname: ''}},
				stuList: [],
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
			uni.request({
				url: gData.site_url + 'User.GetBaseInfo',
				method: 'GET',
				data: {
					'uid': gData.userinfo.id,
					'token': gData.userinfo.token
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
		},
		onLoad(option) {
			let getData = app.globalData;
			console.log(option.courseId)
			uni.request({
				url: getData.site_url + 'CourseCopy.getDetail',
				method: 'GET',
				data: {
					'uid': getData.userinfo.id,
					'token': getData.userinfo.token,
					'courseid': option.courseId
				},
				success: res => {
					if (parseInt(res.data.data.code) !== 0) {
						return;
					}
					this.courseDetail = res.data.data.info;
					this.stuList = res.data.data.info.list
					console.log('课程信息：')
					console.log(this.courseDetail)
					console.log(this.stuList)
				},
			});
			return
			//获取老师基本信息
			if(option.keyword != undefined) {
				this.teacherKeyword = option.keyword;
			}
			this.getTeacherInfo(option.touid);
			// 获取老师课程信息
			this.getTeacherCourse(option.touid);
			
			let gData = app.globalData;
			if(option.touid == gData.userinfo.id) {
				this.is_edt_btn_show = true;
			}
			
		},
		methods: {
			onClick(e) {
				console.log('执行click事件', e.data)
				uni.showToast({
					title: '点击反馈'
				});
			},
			switchChange(e) {
				uni.showToast({
					title: 'change:' + e.value,
					icon: 'none'
				});
			},
			backmycourse() {
				uni.navigateTo({
					url: '../course/mycourse',
				});
			},
			showTeacherInfo(touid) {
				//跳转教师详情页并传入基本信息
				uni.navigateTo({
					url: '../teacherinfo/teacherinfo?touid=' + touid,
				});

			},
			teacherInfo() {
				uni.navigateTo({
					url: "../teacher/teacher"
				})
			},

		}
	}
</script>

<style>
	.top {
		height: 80rpx;
		width: 100%;
		background-color: #0d5e4f;
		display:flex
	}

	.my-item {
		display: flex;
		flex-direction: row;
		align-items: center;
		height: 80rpx;
		margin-top: 20rpx;
		color: #0d5e4f;
	}

	.title-wrap {
		display: block;
		width: 100%;
		height: 61%;
		margin: 0 auto;
	}

	.avatar-wrap {
		background-color: #0d5e4f;
		height: 180rpx;
		padding-top: 80rpx;
	}

	.title-avatar {
		float: left;
	}

	.name-id-wrap {
		width: 79%;
		height: 100%;
		float: left;
		background-color: #0d5e4f;
		position: relative;
		color: #FFFFFF;
		margin-left: 20rpx;
	}

	.name-id-wrap text:first-child {
		display: block;
		font-size: 32rpx;
		font-weight: bold;
	}

	.name-id-wrap text:nth-child(2) {
		display: block;
		margin-top: 10rpx;
	}

	.name-id-wrap text:nth-child(3) {
		position: absolute;
		right: 0rpx;
		top: 30%;
		color: #FFFFFF;
	}

	.id-wrap {
		font-size: 20rpx;
	}

	/* 关注教师 */
	.guan-wrap {
		width: 100%;
		height: 60rpx;
		padding-top: 10rpx;
		padding-left: 20%;
		color: #FFFFFF;
	}

	.guanzhu {
		float: left;
		margin-right: 16rpx;
		font-size: 20rpx
	}

	.guan-info-wrap {
		width: 58%;
		font-size: 20rpx;
		display: inline-block;
	}

	.teacher-info-btn {
		display: inline-block;
		width: 130rpx;
		height: 36rpx;
		line-height: 36rpx;
		text-align: center;
		background-color: transparent;
		color: #FFFFFF;
		border: 2rpx solid #FFFFFF;
		border-radius: 40rpx 40rpx;
		font-size: 20rpx;
	}

	.t-back-img {
		width: 17px;
		height: 17px;
		margin-left: 10px;
		padding-top: 20rpx;
	}

	.classinfo {
		margin: 0 auto;
	}

	.classinfo text{
		font-size: 17px; font-weight: bold; color: #fff; height: 44px;line-height: 44px;
	}
	
	/* ----------------------------以下列表-------------------------- */
	
	.itemline{
		margin-left: 80rpx;
		margin-right: 5rpx;
		height: 2rpx;
		background-color: #cccccc52;
	}

	.userinfo-title-txt {
		margin-left: 20rpx;
		color: #000000;
	}

	.userinfo-icon-img {
		border-radius: 100%;
		width: 80rpx;
		height: 80rpx;
	}

	.uni-icon-wrapper {
		position: absolute;
		right: 20rpx;
	}
	/* ---------------------------以上列表--------------------------- */
</style>
