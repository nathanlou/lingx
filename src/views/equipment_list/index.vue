<template>
	<div class="titles">
		<div class="screen">
			企业：<el-select v-model="company_value" placeholder="请选择" class="select" size='mini' width='10%'>
				<el-option v-for="item in company_list" :key="item.value" :label="item.label" :value="item.value" />
			</el-select>
			关键字：
			<el-input v-model="qhkeyword" placeholder="编号/内部编号" class="select" size='mini' />
			类型：<el-select v-model="style_value" placeholder="请选择" class="select" size='mini'>
				<el-option v-for="item in style_list" :key="item.value" :label="item.label" :value="item.value" />
			</el-select>
			状态：<el-select v-model="status_value" placeholder="请选择" class="select" size='mini'>
				<el-option v-for="item in status_list" :key="item.value" :label="item.label" :value="item.value" />
			</el-select>
			<el-button class="btn" type="primary" icon="el-icon-search" size="mini" round="">搜索</el-button>
			<span style="float: right;">
				<router-link to="/equipment_add/index">
					<el-button class="btn" type="success" icon="el-icon-edit" size="mini" round>添加设备</el-button>
				</router-link>
				<router-link to="/batch_add/index">
					<el-button class="btn" type="warning" icon="el-icon-edit" size="mini" round>批量添加</el-button>
				</router-link>
				<el-button class="btn" type="danger" icon="el-icon-delete" size="mini" @click="deletes()" round>批量删除</el-button>
			</span>
		</div>
		<div class="container_table">
			<div class="table_headr">列 表</div>
			<el-table ref="multipleTable" :data="tableData.slice((currentPage-1)*pagesize,currentPage*pagesize)" border stripe
			 style="width: 100%;font-size: 13px;" :default-sort="{prop: 'date', order: 'descending'}" :row-style="{height: '0'}"
			 :cell-style="{padding: '0'}" size='mini'>
				<el-table-column fixed type="selection" align="center" size='mini' />
				<el-table-column fixed prop="bh" sortable label="设备编号" align="center" width="150" size='mini' />
				<el-table-column prop="nbbh" sortable label="内部编号" align="center" width="115" size='mini' />
				<el-table-column prop="gdmc" label="工地名称" width="100" align="center" size='mini' />
				<el-table-column prop="sbzl" label="设备种类" align="center" size='mini' />
				<el-table-column prop="sbxh" label="设备型号" align="center" size='mini' />
				<el-table-column prop="yxsj" sortable label="运行时间" align="center" width="110" size='mini' />
				<el-table-column prop="qdcs" sortable label="启动次数" align="center" width="110" size='mini' />
				<el-table-column prop="sbzt" label="设备状态" align="center" size='mini'>
					<template slot-scope="scope">
						<el-tag :type="scope.row.sbzt === '在线' ? 'primary' : 'danger'" disable-transitions size="small">{{ scope.row.sbzt }}</el-tag>
					</template>
				</el-table-column>
				<el-table-column prop="kjzt" label="开机状态" align="center">
					<template slot-scope="scope">
						<el-tag :type="scope.row.kjzt === '开机' ? 'primary' : 'danger'" disable-transitions size="small">{{ scope.row.kjzt }}</el-tag>
					</template>
				</el-table-column>
				<el-table-column label="操作" width="280" align="center" fixed="right">
					<template slot-scope="scope">
						<el-button class="btn btn_do" type="success" size="mini" @click="monitor(scope.$index, scope.row)" round plain>监控</el-button>
						<el-button class="btn btn_do" type="warning" size="mini" @click="set_up(scope.$index, scope.row)" round plain>设置</el-button>
						<el-button class="btn btn_do" type="danger" size="mini" round plain>二维码</el-button>
					</template>
				</el-table-column>
			</el-table>
			<el-pagination class="fy" layout="total,prev, pager, next" :total="total" background @current-change="current_change" />
		</div>
	</div>
</template>

<script>
	export default {
		data() {
			return {
				total: 1000, // 默认数据总数
				pagesize: 10, // 每页的数据条数
				currentPage: 1, // 默认开始页面
				istag: true,
				tableData: [{
						bh: 'QHW001G210290201',
						nbbh: 'qh001',
						gdmc: '新乡火炬园',
						sbzl: 'YLY1',
						sbxh: '1型',
						yxsj: '1410',
						qdcs: '1236',
						sbzt: '在线',
						kjzt: '开机'
					},
					{
						bh: 'QHW001G210290202',
						nbbh: 'qh002',
						gdmc: '新乡火炬园',
						sbzl: 'YLY1',
						sbxh: '1型',
						yxsj: '14110',
						qdcs: '126',
						sbzt: '离线',
						kjzt: '关机'
					}
				],
				company_list: [{
					value: '选项0',
					label: '全部'
				}, {
					value: '选项1',
					label: '潜合测试'
				}, {
					value: '选项2',
					label: '客户体验'
				}],
				style_list: [{
					value: '选项0',
					label: '全部'
				}, {
					value: '选项1',
					label: 'YLY1'
				}, {
					value: '选项2',
					label: '智能除尘'
				}],
				status_list: [{
					value: '选项0',
					label: '全部'
				}, {
					value: '选项1',
					label: '在线'
				}, {
					value: '选项2',
					label: '离线'
				}],
				company_value: '',
				style_value: '',
				status_value: '',
				qhkeyword: ''
			}
		},
		created: function() {
			this.total = this.tableData.length
		},
		methods: {
			set_up(index, row) {
				this.$router.push({
					path: 'set_up',
					query: {
						key: row
					}
				})
			},
			monitor(index, row) {
				this.$router.push({
					path: 'monitor',
					query: {
						key: row
					}
				})
			},
			current_change: function(currentPage) {
				this.currentPage = currentPage
			},
			deleteRow(index, rows) {
				rows.splice(index, 1)
			},
			deletes() {
				this.selectionDatas = []
				var arr = []
				var data = this.$refs.multipleTable.selection
				if (data.length == 0) {
					this.$alert('您没有任何选择', '提示', {
						confirmButtonText: '确定',
						callback: action => {}
					})
				} else {
					data.forEach(function(item) {
						arr.push(item.bh)
					})
					this.$confirm('此操作将永久删除该文件, 是否继续?', '提示', {
						confirmButtonText: '确定',
						cancelButtonText: '取消',
						type: 'warning'
					}).then(() => {
						this.$message({
							type: 'success',
							message: '删除成功!'
						})
					}).catch(() => {
						this.$message({
							type: 'info',
							message: '已取消删除'
						})
					})
				}

				// 				request.delete(apis.manageModel, arr)
				// 					.then(function(response) {
				// 						if (response.data.code == 200) {
				//
				// 						}
				// 					})
				// 					.catch(function(error) {
				// 						console.log(error);
				// 					});
			}
		}
	}
</script>

<style scoped="scoped">
	.screen {
		font-size: 0.875rem;
		color: gray;
	}

	.select {
		width: 10rem;
		margin-right: 1rem;
	}

	.fy {
		text-align: right;
		margin-top: 0.625rem;
	}

	.titles {
		width: 95%;
		margin-left: 1rem;

		height: 100%;
		margin-bottom: 1.25rem;
		font-size: 0.875rem;
	}

	.btn {
		margin-left: 0.5rem;
		margin-bottom: 0.625rem;

	}

	.btn_do {
		margin-top: 0.625rem;

	}

	.container_table {
		clear: both;
	}

	.table_headr {
		height: 2rem;
		line-height: 2rem;
		color: white;
		background-color: #409EFF;

	}

	@media screen and (max-width: 1024px) {
		.titles {
			font-size: 0.875rem;
			margin-top: -2.5%;
		}

		.select {
			width: 6.7rem;

			margin-right: 1rem;
		}

		.btn {
			margin-top: 0.625rem;
		}
	}
</style>
