<template>
	<view>
		<!-- #ifdef MP-WEIXIN -->
		<cu-custom    :isBack="true">
			<block slot="backText">返回</block>
			<block slot="content">活动详情</block>
		</cu-custom>
		<!-- #endif -->
		<view class="margin-left cu-bar tabbar">
			<view>
				<image :src="activity.actions[0].img1" style="width: 400rpx; height: 60rpx;"></image>
			</view>
			<view class="action">
				<view class="cuIcon-cu-image">
				</view>
				<view class="text-gray"></view>
			</view>
			<view class="action">
				<view class="cuIcon-cu-image" @tap="goHome">
					<image :src="activity.home_icon" style="width: 50rpx; height: 50rpx;"></image>
				</view>
				<view class="">首页</view>
			</view>
		</view>
		<view class="margin-left" style="margin-top:-30rpx;">
			<view class="text-bold text-lg">精益求精 睿智精选</view>
			<view class="text-bold">"十分精选"定制产品交流会</view>
		</view>

		<view class="padding-xs text-center margin-top solid-top">
			<view class="text-lg padding-xs">
				<texxt class="margin-top text-bold text-lg ">☆  温馨提示  ☆</texxt>
			</view>
		</view>
		<view class="margin-left margin-right padding-bottom">
			<view>尊敬的各位代表：</view>
			<text class="text-box  padding-sm " space="nbsp" scroll-y="true">
				<text class="margin-left-lg"></text>
				为便于您在会议期间集中精力参加会议，现将有关会务情况介绍如下，期望得到您的支持与配合。
			</text>
		</view>

		<view class="margin-left margin-right padding-bottom">
			<text class="text-box  padding-sm  text-yellow" scroll-y="true">
				1、为配合上海市防疫规定，请在出行前携带好口罩，酒店房间配有简单防疫套装，如需请自行取用；
			</text>
		</view>

		<view class="margin-left margin-right padding-bottom">
			<text class="text-box  padding-sm  text-yellow" scroll-y="true">
				2、为确保会议效果，请所有与会代表严格遵守会议纪律，着正装提前15分钟到达会场就座。会议期间，请关闭通讯工具或调成静音，严禁在会议室和房间内吸烟；
			</text>
		</view>

		<view class="margin-left margin-right padding-bottom">
			<text class="text-box  padding-sm  text-yellow" scroll-y="true">
				3、为了您的安全，请务必关好门窗，妥善保管随身物品和文件资料；
			</text>
		</view>

		<view class="margin-left margin-right padding-bottom">
			<text class="text-box  padding-sm  text-yellow" scroll-y="true">
				4、参会代表最迟于5月21日12:00前至酒店前台办理退房；
			</text>
		</view>

		<view class="margin-left margin-right padding-bottom">
			<text class="text-box  padding-sm  text-yellow" scroll-y="true">
				5、为及时掌握会议动态，请与会代表随时注意会务组通知。
			</text>
		</view>
		<view class="margin ">
			<image :src="activity.actions[2].img1" mode="aspectFill" class="radius" style="width: 372px; height: 400px;"></image>
		</view>
		<view class="margin-left margin-right padding">
			<view class="text-bold">注意事项</view>
			<text class="margin-top text-box  padding-sm " space="nbsp" scroll-y="true">
				<text style="margin-left:-18rpx;"></text>
				为确保会议效果，烦请与会代表遵守会议纪律，着正装提前15分钟到达会场，会议期间请关闭通讯工具或将其调至静音模式。
				为了您的安全，请务必管好门窗并妥善保管随身物品和文件资料。
			</text>
		</view>
		<view class="margin-left margin-right padding">
			<view class="text-bold">防疫提示</view>
			<text class="margin-top text-box  padding-sm " space="nbsp" scroll-y="true">
				<text style="margin-left:-18rpx;"></text>
				鉴于当前疫情防控形势依然严峻，为有效控制和降低信管疫情传播风险。
				请各位与会领导们在会议期间随身携带口罩并适时佩戴。
				保持社交距离，做好个人防护工作。
				进出公共场所时请提前准备并出示健康绿码。

				感谢各位与会领导的支持与配合！
			</text>
		</view>
		<view class="margin-left-xxl margin-right-xxl flex align-center" >
			<image :src="activity.logo" mode="aspectFill" class="padding-xxl margin-left-xxl margin-right-xxl" style="width: 100%;"></image>
		</view>
		<view class="cu-tabbar-height"></view>
		<view class="cu-tabbar-height"></view>
	</view>
</template>

<script>
    import { mapState } from 'vuex'
    // #ifdef H5
    import cloudbase from '@cloudbase/js-sdk';
    // #endif

    export default {
        data() {
            return {
                activity:{
                    _id:'',
                    qrcode:'',
                    content:'',
                    actions:[]

                },
            }
        },
        onLoad(options) {
            let that = this;
            if(options.id) {
                that.activity._id = options.id;
                that.getDetail();
            }
        },

        onShareAppMessage(res){
            if(res.from=== 'button'){
                let  activity = res.target.dataset.activity;
                return{
                    title:activity.title,
                    imageUrl:activity.cover||'',
                    path:"/pagesCmn/activity/ShareActivityDetail?id="+activity._id,
                    success:function(r){
                        uni.showToast({
                            'icon':'success',
                            'title':'分享成功'
                        });
                    }
                }
            }
        },
        computed:{
            ...mapState(['user']),
        },
        methods: {
            async getDetail(){
                let that = this
                let res = null;
                // #ifdef MP-WEIXIN
                res = await wx.cloud.callFunction({
                    name:'cmn',
                    data:{
                        $url:'getActivityDetail',
                        id:that.activity._id
                    }
                });
                that.activity = res.result.data;
                // #endif
                // #ifdef H5
                res = await cloudbase.callFunction({
                    name:'web',
                    data:{
                        $url:'getActivityDetail',
                        id:that.activity._id
                    }
                });
                that.activity = JSON.parse(res.result.data);
                // #endif

                let parseSignUp = that.activity.sign_up.date+' '+that.activity.sign_up.time;
                let parseSignIn = that.activity.sign_in.date+' '+that.activity.sign_in.time;
                that.activity.sign_up = parseSignUp;
                that.activity.sign_in = parseSignIn;
            },



            goHome(){
                uni.navigateBack();
            },
        }
    }
</script>

<style>
</style>
