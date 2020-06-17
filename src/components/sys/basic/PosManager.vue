<template>
	<div>
		<div>
			<el-input style="width: 300px;" placeholder="添加职位" prefix-icon="el-icon-plus" v-model="pos.name">
			</el-input>
			<el-button style="margin-left: 10px;" icon="el-icon-plus" type="primary" @click="handleAdd">添加</el-button>
			<el-button type="danger" :disabled="disable" @click="batchDelete">批量删除</el-button>
		</div>
		<div>
			<el-table :data="positions" border stripe fit @selection-change="handleSelectionChange">
				style="width: 60%;margin-top: 10px;">
				<el-table-column type="selection" width="55">
				</el-table-column>
				<el-table-column prop="id" label="编号" align="center">
				</el-table-column>
				<el-table-column prop="name" label="职位名称" align="center">
				</el-table-column>
				<el-table-column prop="createDate" label="创建时间" align="center">
				</el-table-column>
				<el-table-column label="操作" align="center">
					<template slot-scope="scope">
						<el-button size="small" @click="showEditView(scope.$index, scope.row)">编辑</el-button>
						<el-button size="small" type="danger" @click="handleDelete(scope.$index, scope.row)">删除</el-button>
					</template>
				</el-table-column>
			</el-table>

		</div>
		<el-dialog title="修改职位" :visible.sync="dialogVisible" width="30%">
			<div>
				<el-tag>职位名称</el-tag>
				<el-input style="width: 200px;margin-left: 8px;" size="small" v-model="updatePos.name"></el-input>
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
		name: 'PosManager',
		data() {
			return {
				pos: {
					name: '',
				},
				positions: [],
				dialogVisible: false,
				updatePos: {
					name: ''
				},
				ids:[],
				disable: true
			}
		},
		methods: {
			batchDelete(){
				this.$confirm('此操作将永久删除【' + this.ids.length + '】条记录, 是否继续?', '提示', {
					confirmButtonText: '确定',
					cancelButtonText: '取消',
					type: 'warning'
				}).then(() => {
					this.deleteRequest("/system/basic/pos/",this.ids).then(res => {
						if(res){
							this.initPosition()
						}
					})
				}).catch(() => {
					this.$message({
						type: 'info',
						message: '已取消删除'
					});
				});
			},
			handleSelectionChange(batch) {
				this.ids.length=0
				batch.forEach(item=>{
					this.ids.push(item.id)
				})
				if(this.ids.length>0){
					this.disable=false
				}else{
					this.disable=true
				}
			},
			doUpdate() {
				this.putRequest("/system/basic/pos/", this.updatePos).then(res => {
					if (res) {
						this.initPosition()
						this.dialogVisible = false
					}
				})
			},
			handleAdd() {
				if (this.pos.name) {
					this.postRequest("/system/basic/pos/", this.pos).then(res => {
						this.initPosition()
						this.pos.name = ''
					})
				} else {
					this.$message.error('职位不能为空！');
				}
			},
			showEditView(index, data) {
				Object.assign(this.updatePos, data);
				this.dialogVisible = true
			},
			handleDelete(index, data) {
				this.$confirm('此操作将永久删除' + data.name + '职位, 是否继续?', '提示', {
					confirmButtonText: '确定',
					cancelButtonText: '取消',
					type: 'warning'
				}).then(() => {
					this.deleteRequest("/system/basic/pos/" + data.id).then(res => {
						this.initPosition()
					})
				}).catch(() => {
					this.$message({
						type: 'info',
						message: '已取消删除'
					});
				});
			},
			initPosition() {
				this.getRequest("/system/basic/pos/").then(res => {
					this.positions = res
				})
			}
		},
		mounted() {
			this.initPosition()
		}
	}
</script>

<style>
</style>
