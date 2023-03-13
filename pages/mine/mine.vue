<template>
	<view class="mine-box">
		<view class="jieguo">
			<uni-section title="历史结果" type="line">
				<uni-list>
					<uni-list-item :title="item.mingZi" :note="item.text" :rightText="`《${item.type}》`" v-for="(item, index) in listData" :key="index" />
				</uni-list>
			</uni-section>
		</view>
		<ad unit-id="4992cc5e7f5baaa6aba326e07a036d8e"></ad>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				listData: [],
			}
		},
		onShow: function (option) {
			if(qq.getStorageSync("qumingshenqijieguo")){
				this.listData = JSON.parse(qq.getStorageSync("qumingshenqijieguo"));
			}
		},
		methods: {
			
		}
	}
</script>

<style lang="scss">
.mine-box{
	padding: 10px;
	.jieguo{
		margin-top: 5px;
		margin-bottom: 30px;
	}
}
</style>
