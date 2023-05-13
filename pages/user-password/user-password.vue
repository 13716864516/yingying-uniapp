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
		<input class="uni-input" type="text" value="" placeholder="输入旧密码" v-model="oldpassword"/>
		<input class="uni-input" type="text" value="" placeholder="输入新密码" v-model="newpassword" />
		<input class="uni-input" type="text" value="" placeholder="输入确认密码" v-model="renewpassword" />
		
		<view class="py-2 px-2">
			<button class="bg-main text-white" style="border-radius: 50rpx; border: 0;" 
			type="primary" :disabled="disabled" @click="submit">设置</button>
		</view>
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				oldpassword:"",
				newpassword:"",
				renewpassword:"",
			}
		},
		computed:{
			disabled(){
				return this.oldpassword == '' || this.newpassword == '' || this.renewpassword == '';
			}
		},
		methods: {
			// 验证
			check(){
				if(this.newpassword !== this.renewpassword){
					uni.showToast({
						title: '两次密码输入不一致'
					});
					return false;
				}
				return true;
			},
			submit(){
				if(!this.check()){
					return;
				}

			}
		}
	}
</script>

<style>

</style>
