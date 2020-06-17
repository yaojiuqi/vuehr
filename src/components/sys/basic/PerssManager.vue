<template>
	<div>
		<div>
			<el-input placeholder="请输入角色英文名" v-model="role.name" style="width: 20%;">
				<template slot="prepend">ROLE_</template>
			</el-input>
			<el-input placeholder="请输入中文名" v-model="role.nameZh" style="width: 20%; padding-left: 20px;padding-right: 20px;"></el-input>
			<el-button type="primary" icon="el-icon-plus" @click="addRole">添加角色</el-button>
		</div>
		<div style="width: 60%; margin-top: 20px;">
			<el-collapse accordion @change="change">
				<el-collapse-item v-for="(r,index) in roles" :title="r.nameZh" :name="r.id" :key="index">
					<el-card class="box-card">
						<div slot="header" class="clearfix">
							<span>可访问的资源</span>
							<el-button style="float: right; padding: 3px 0;color:chocolate;" icon="el-icon-delete" type="text" @click="deleteRole(r)"></el-button>
						</div>
						<div>
							<el-tree ref="tree" :data="allMenus" :props="defaultProps" show-checkbox node-key="id" :default-checked-keys="checkedMenus"
							 :key="index"></el-tree>
							<div style="display: flex;justify-content: flex-end;">
								<el-button size="small" type="primary" @click="doUpdate(r.id,index)">确认修改</el-button>
							</div>
						</div>
					</el-card>
				</el-collapse-item>
			</el-collapse>
		</div>
	</div>
</template>

<script>
	export default {
		name: "PerssManager",
		data() {
			return {
				role: {
					name: '',
					nameZh: ''
				},
				roles: [],
				allMenus: [],
				defaultProps: {
					children: 'children',
					label: 'name'
				},
				checkedMenus: []
			}
		},
		methods: {
			deleteRole(role) {
				this.$confirm('此操作将永久删除【' + role.nameZh + '】角色, 是否继续?', '提示', {
					confirmButtonText: '确定',
					cancelButtonText: '取消',
					type: 'warning'
				}).then(() => {
					this.deleteRequest("/system/basic/permiss/menus/" + role.id).then(res => {
						if (res) {
							this.initRoles()
						}
					})
				}).catch(() => {
					this.$message({
						type: 'info',
						message: '已取消删除'
					});
				});
			},
			doUpdate(id, index) {
				let selectedKeys = this.$refs.tree[index].getCheckedKeys(true)
				this.putRequest("/system/basic/permiss/menus/" + id, selectedKeys).then(res => {
					this.initSelectMenus(id)
				})
			},
			change(id) {
				if (id) {
					this.initAllMenus()
					this.initSelectMenus(id)
				}
			},
			initAllMenus() {
				this.getRequest("/system/basic/permiss/menus").then(res => {
					this.allMenus = res
				})
			},
			initSelectMenus(id) {
				this.checkedMenus=[]
				this.getRequest("/system/basic/permiss/menus/" + id).then(res => {
					this.checkedMenus = res
				})
			},
			addRole() {
				if (this.role.name && this.role.nameZh) {
					this.postRequest("/system/basic/permiss/", this.role).then(res => {
						if (res) {
							this.initRoles()
							this.role.name = ''
							this.role.nameZh = ''
						}
					})
				} else {
					this.$message.error("数据不能为空！")
				}
			},
			initRoles() {
				this.getRequest("/system/basic/permiss/").then(res => {
					this.roles = res
				})
			}

		},
		mounted() {
			this.initRoles()
		}
	}
</script>

<style>
</style>
