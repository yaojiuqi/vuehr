<template>
	<div style="width: 700px;">
		<el-input placeholder="请输入部门名称进行搜索" v-model="filterText" prefix-icon="el-icon-search">
		</el-input>

		<el-tree :data="deps" :props="defaultProps" :filter-node-method="filterNode" ref="tree" default-expand-all>
			<span class="custom-tree-node" slot-scope="{ node, data }" style="display: flex;justify-content: space-between;width: 100%;">
				<span>{{ node.label }}</span>
				<span>
					<el-button type="primary" size="mini" style="padding: 5px;" @click.stop="() => showAddView(data)">
						添加部门
					</el-button>
					<el-button type="danger" size="mini" style="padding: 5px;" @click.stop="() => deleteDep(data)">
						删除部门
					</el-button>
				</span>
			</span>
		</el-tree>
	</div>
</template>

<script>
	export default {
		name: "DepManager",
		data() {
			return {
				filterText: '',
				defaultProps: {
					children: 'children',
					label: 'name'
				},
				deps: []
			}
		},
		watch: {
			filterText(val) {
				this.$refs.tree.filter(val);
			}
		},
		mounted() {
			this.initDeps()
		},
		methods: {
			showAddView(data){
				console.log(data)
			},
			deleteDep(data){
				console.log(data)
			},
			initDeps() {
				this.getRequest("/system/basic/dept/").then(res => {
					if (res) {
						this.deps = res
					}
				})
			},
			filterNode(value, data) {
				if (!value) return true;
				return data.name.indexOf(value) !== -1;
			}
		}

	}
</script>

<style>
</style>
