<template>
	<div>
		<el-container>
		  <el-header class="homeHeader">
			  <div class="title">微人事</div>
			  <el-dropdown class="userinfo" @command="commandHandler">
			    <span class="el-dropdown-link">
			      {{user.name}}<i><img :src="user.userface" alt=""></i>
			    </span>
			    <el-dropdown-menu slot="dropdown">
			      <el-dropdown-item command="userinfo">个人中心</el-dropdown-item>
			      <el-dropdown-item command="setting">设置</el-dropdown-item>
			      <el-dropdown-item command="logout" divided>注销登录</el-dropdown-item>
			    </el-dropdown-menu>
			  </el-dropdown>
		  </el-header>
		  <el-container>
		    <el-aside width="200px">Aside</el-aside>
		    <el-main>Main</el-main>
		  </el-container>
		</el-container>
	</div>
</template>

<script>
	export default{
		data(){
			return {
				user: JSON.parse(window.sessionStorage.getItem("user"))
			}
		},
		methods:{
			commandHandler(val){
				if(val == 'logout'){
					 this.$confirm('此操作将注销登录, 是否继续?', '提示', {
					          confirmButtonText: '确定',
					          cancelButtonText: '取消',
					          type: 'warning'
					        }).then(() => {
					          this.getRequest("/logout");
							  window.sessionStorage.removeItem("user");
							  this.$router.replace('/');
					        }).catch(() => {
					          this.$message({
					            type: 'info',
					            message: '已取消操作'
					          });          
					        });
				}
			}
			
		}
	}
</script>

<style>
	.homeHeader{
		/* 设置背景颜色 */
		background-color: mediumturquoise;
		/* 弹性布局 */
		display: flex;
		/* 垂直居中 */
		align-items: center;
		/* 两端对齐 */
		justify-content: space-between;
		padding: 0px 15px;
		box-sizing: border-box;
	}
	.homeHeader .title{
		font-size: 30px;
		font-family: 华文行楷;
		color: #FFF;
	}
	.userinfo{
		cursor: pointer;
	}
	.el-dropdown-link{
		display: flex;
		align-items: center;
	}
	.el-dropdown-link img{
		width: 40px;
		height: 40px;
		margin-left: 10px;
		border-radius: 20px;
	}
</style>
