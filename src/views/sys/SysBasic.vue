<template>
	<div>
		<el-tabs v-model="activeName" type="card">
		    <el-tab-pane label="部门管理" name="depManager"><DepManager/></el-tab-pane>
		    <el-tab-pane label="职位管理" name="posManager"><PosManager/></el-tab-pane>
		    <el-tab-pane label="职称管理" name="jobLevelManager"><JobLevelManager/></el-tab-pane>
		    <el-tab-pane label="奖惩规则" name="ecManager"><EcManager/></el-tab-pane>
		    <el-tab-pane label="权限组" name="perssManager"><PerssManager/></el-tab-pane>
		  </el-tabs>
	</div>
</template>
<script>
	import DepManager from '@/components/sys/basic/DepManager'
	import PosManager from '@/components/sys/basic/PosManager'
	import JobLevelManager from '@/components/sys/basic/JobLevelManager'
	import EcManager from '@/components/sys/basic/EcManager'
	import PerssManager from '@/components/sys/basic/PerssManager'
	export default{
		name:'SysBasic',
		data(){
			return{
				activeName: "depManager"
			}
		},
		components:{
			DepManager,
			PosManager,
			JobLevelManager,
			EcManager,
			PerssManager
		}
	}
</script>

<style>
</style>
