<template>
	<view class="container">
		<view class="content">
			<view class="title">老话说：“男诗经，女楚辞，文论语，武周易”。</view>
			<view class="">
				<uni-forms :model="formData" ref="form" :rules="rules">
					<uni-forms-item label="姓氏" name="surname" required>
						<uni-easyinput type="text" v-model="formData.surname" placeholder="请输入姓名" />
					</uni-forms-item>
					<uni-forms-item label="字数" name="wordLength" required>
						<uni-data-checkbox v-model="formData.wordLength" :localdata="wordLengths" />
					</uni-forms-item>
					<uni-forms-item label="来源" name="sources" required>
						<uni-data-checkbox v-model="formData.sources" multiple :localdata="sources" />
					</uni-forms-item>
				</uni-forms>
				<button class="button" @click="submit">
					开始择名
				</button>
				<view class="tip">*PS：每次择名都需观看广告，最多3次，3次后不再弹出广告</view>
			</view>
		</view>
		
		<ad unit-id="845a1f79d90a8e6e807651fc3574d9dc" type="card"></ad>
	</view>
</template>

<script>
	
	export default {
		data() {
			return {
				wordLengths: [
					{
						text:'一个字',
						value: '1'
					},
					{
						text:'两个字',
						value: '2'
					},
				],
				sources: [
					{
						text:'名句',
						value: 'MJ'
					},
					{
						text:'诗经',
						value: 'SJ'
					},
					{
						text:'楚辞',
						value: 'CC'
					},
					{
						text:'论语',
						value: 'LY'
					},
					{
						text:'周易',
						value: 'ZY'
					}
				],
				formData: {
					surname: '',
					wordLength: '1',
					sources: [],
				},
				rules: {
					surname: {
						rules: [{
							required: true,
							errorMessage: '姓氏不能为空'
						}]
					},
					wordLength: {
						rules: [{
							required: true,
							errorMessage: '字数不能为空'
						}]
					},
					sources: {
						rules: [{
							required: true,
							errorMessage: '来源不能为空'
						}]
					},
				},
			}
		},
		created(){
			qq.setStorageSync("qumingshenqi", 0);
		},
		methods: {
			ad(){
				var num = qq.getStorageSync("qumingshenqi");
				let videoAd = qq.createRewardedVideoAd({
				  adUnitId: '6ecde0dcd6eb2a327f7410b9342496a4'
				})
				
				videoAd.onError((res)=>{
				  console.log('videoAd onError',res)
				})
				videoAd.onLoad((res)=>{
				  console.log('videoAd onLoad',res)
				})
				videoAd.onClose((res)=>{
					console.log(res)
					if(res.isEnded){
				        console.log('观看完成')
						qq.setStorageSync("qumingshenqi", num+=1);
				        this.tz()
					}else{
						// console.log('半途而废')
						// Taro.showModal({
						//    title:"未观看完成",
						//    content:"请支持开发者维护这个应用，看完后，会自动提交！"
						// })
					}
					// videoAd.offClose();
					// videoAd.offError();
					// videoAd.offLoad();
				})
				
				videoAd.load().then(() => {
				    console.log('激励视频加载成功');
				    videoAd.show().then(() => {
				      console.log('激励视频 广告显示成功')
				    })
				    .catch(err => {
				      console.log('激励视频 广告显示失败')
				    })
				  })
				.catch(err => {
				    console.log('激励视频加载失败');
				})
			},
			submit() {
				this.$refs.form.validate().then(res => {
					const num = qq.getStorageSync("qumingshenqi");
					if(num<3){
						// 广告
						this.ad()
						return
					}else{
						this.tz()
					}
					
				}).catch(err => {
					console.log('err', err);
				})
			},
			tz(){
				uni.navigateTo({
					url: '/pages/detail/detail?param='+encodeURIComponent(JSON.stringify(this.formData))
				});
			}
		}
	}
</script>

<style lang="scss">
	.container {
		background-color: #ccca9b;
		font-size: 14px;
		line-height: 24px;
		color: #000;
		.content{
			padding: 20px;
		}
		.title{
			width: 100%;
			text-align: center;
			margin-bottom: 20px;
		}
		button{
			color: #fff;
			background: #80683e;
			border: 1px solid #fff;
		}
		.tip{
			font-size: 12px;
			color:#3f789e;
		}
	}
</style>
