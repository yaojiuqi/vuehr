<template>
	<div>
		<div>
			<el-input v-model="jl.name" style="width: 300px;" placeholder="添加职称" prefix-icon="el-icon-plus"></el-input>
			<el-select v-model="jl.titleLevel" placeholder="职称等级" style="margin-left: 10px;margin-right: 10px;">
				<el-option v-for="(item,index) in titleLevels" :key="index" :label="item" :value="item">
				</el-option>
			</el-select>
			<el-button icon="el-icon-plus" type="primary" @click="addJobLevel">添加</el-button>
			<el-button type="danger" :disabled="disable" @click="batchDelete">批量删除</el-button>
		</div>
		<div>
			<el-table :data="jls" border stripe fit style="width: 80%;margin-top: 10px;" @selection-change="handleSelectionChange">
				<el-table-column type="selection" width="55">
				</el-table-column>
				<el-table-column prop="id" label="编号" align="center">
				</el-table-column>
				<el-table-column prop="name" label="名称" align="center">
				</el-table-column>
				<el-table-column prop="titleLevel" label="级别" align="center">
				</el-table-column>
				<el-table-column prop="createDate" label="创建时间" align="center">
				</el-table-column>
				<el-table-column label="是否启用" align="center">
					<template slot-scope="scope">
						<el-tag type="success" v-if="scope.row.enabled">已启用</el-tag>
						<el-tag type="warning" v-else>未启用</el-tag>
					</template>
				</el-table-column>
				<el-table-column label="操作" align="center">
					<template slot-scope="scope">
						<el-button size="small" @click="showEditView(scope.row)">编辑</el-button>
						<el-button size="small" type="danger" @click="handleDelete(scope.row)">删除</el-button>
					</template>
				</el-table-column>
			</el-table>
		</div>
		<el-dialog title="修改职称" :visible.sync="dialogVisible" width="30%">
			<div>
				<el-form :model="updateJl" label-width="80px">
					<el-form-item label="职位名称">
						<el-input v-model="updateJl.name"></el-input>
					</el-form-item>
					<el-form-item label="职称等级">
						<el-select v-model="updateJl.titleLevel" style="width: 100%;">
							<el-option v-for="(item,index) in titleLevels" :key="index" :label="item" :value="item">
							</el-option>
						</el-select>
					</el-form-item>
					<el-form-item label="是否可用">
						<el-switch v-model="updateJl.enabled"></el-switch>
					</el-form-item>
				</el-form>
			</div>
			<span slot="footer" class="dialog-footer">
				<el-button size="small" @click="dialogVisible = false">取 消</el-button>
				<el-button size="small" type="primary" @click="doUpdate">确 定</el-button>
			</span>
		</el-dialog>
	</div>
</template>

<script>
	export default {
		name: "JobLevelManager",
		data() {
			return {
				jl: {
					name: '',
					titleLevel: ''
				},
				jls: [],
				ids: [],
				titleLevels: [
					'正高级',
					'副高级',
					'高级',
					'中级',
					'初级',
					'员级'
				],
				disable: true,
				dialogVisible: false,
				updateJl: {
					name: '',
					titleLevel: '',
					enabled: false
				}

			}
		},
		methods: {
			doUpdate(){
				this.putRequest("/system/basic/joblevel/",this.updateJl).then(res=>{
					if(res){
						this.dialogVisible=false
						this.initJls()
					}
				})
			},
			showEditView(data) {
				Object.assign(this.updateJl,data)
				this.dialogVisible = true
			},
			batchDelete() {
				this.$confirm('此操作将永久删除【' + this.ids.length + '】条记录, 是否继续?', '提示', {
					confirmButtonText: '确定',
					cancelButtonText: '取消',
					type: 'warning'
				}).then(() => {
					this.deleteRequest("/system/basic/joblevel/", this.ids).then(res => {
						this.initJls()
					})
				}).catch(() => {
					this.$message({
						type: 'info',
						message: '已取消删除'
					});
				});
			},
			handleSelectionChange(data) {
				this.ids.length = 0
				data.forEach(item => {
					this.ids.push(item.id)
				})
				if (this.ids.length > 0) {
					this.disable = false
				} else {
					this.disable = true
				}
			},
			handleDelete(data) {
				this.$confirm('此操作将永久删除【' + data.name + '】记录, 是否继续?', '提示', {
					confirmButtonText: '确定',
					cancelButtonText: '取消',
					type: 'warning'
				}).then(() => {
					this.deleteRequest("/system/basic/joblevel/" + data.id).then(res => {
						this.initJls()
					})
				}).catch(() => {
					this.$message({
						type: 'info',
						message: '已取消删除'
					});
				});
			},
			addJobLevel() {
				if (this.jl.name && this.jl.titleLevel) {
					this.postRequest("/system/basic/joblevel/", this.jl).then(res => {
						this.initJls()
						this.jl.name = ''
						this.jl.titleLevel = ''
					})
				} else {
					this.$message.error("添加名称或等级不能为空！")
				}
			},
			initJls() {
				this.getRequest("/system/basic/joblevel/").then(res => {
					this.jls = res
				})
			}
		},
		mounted() {
			this.initJls()
		}

	}
</script>

<style>
</style>
