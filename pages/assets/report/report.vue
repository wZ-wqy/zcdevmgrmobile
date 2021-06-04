<template>
	<view class="wrap">
		<u-cell-group >
			<view v-for="(item, index) in list" :key="index">
				<u-cell-item :title="item.name" :label="item.remark" @click="openPage(item.path)"> 
				</u-cell-item>
				<u-line></u-line>
			</view>
		</u-cell-group>
	</view>
 
</template>

<script>
	export default {
		data() {
			return {
			  list:[
			  {name:"资产所属公司统计表",path:"pages/assets/report/belongcomp/belongcomp"},
			  {name:"资产使用公司统计表",path:"pages/assets/report/usedcomp/usedcomp"},
			  {name:"资产使用部门统计表",path:"pages/assets/report/usedpart/usedpart"},
			  {name:"资产分类统计表",path:"pages/assets/report/zccategory/zccategory"}
			  ]
			}
		},
		onLoad() {
		},
		methods: {
			openPage(path){
				this.$u.route({
					url:path,
				});
			}
		}	
	}
</script>

<style>

</style>
