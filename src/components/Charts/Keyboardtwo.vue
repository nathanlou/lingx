<template>
	<div style="width: 95%;margin-left: 2.5%;">
		<div style="display: flex;">
		<div style="width: 60%;">
			<div class="screen">
				企业：<el-select v-model="company_value" placeholder="请选择" class="select">
					<el-option v-for="item in company_list" :key="item.value" :label="item.label" :value="item.value" />
				</el-select>
				关键字：
				<el-input v-model="qhkeyword" placeholder="编号、内部编号" class="select" />
				<el-button class="btn" type="primary" icon="el-icon-search" size="mini">搜索</el-button>
			</div>
			<div style="display: flex;">
				<div class="sbzk" v-for="(item,index) in sbzk" :key='index'>
					<div>{{item.name}}</div>
					<div class="bigtitle">{{item.value}}</div>
					<div class="text"><i class="el-icon-top" style="color: red;"></i>{{item.bl}}%</div>
				</div>
			</div>
			<div id="map" style="width: 100%;height: 80vh;top: -50px;"></div>
	</div>
	<div style="width: 40%;">
		<div class="block">
			<el-date-picker v-model="value1" type="date" placeholder="选择日期">
			</el-date-picker>
		</div>
		<el-table ref="multipleTable" :data="tableData" border stripe
		 style="width: 100%;font-size: 13px;margin-top: 1.5%;" :default-sort="{prop: 'date', order: 'descending'}">
			<el-table-column prop="czzh" label="排名" align="center" />
			<el-table-column prop="cznr" label="地区" align="center" />
			<el-table-column prop="czip" label="设备数量" align="center" />
			<el-table-column prop="czsj" label="占比" align="center" />
		</el-table>
	</div>

	</div>
	</div>
</template>

<script>
	import echarts from 'echarts'
	import resize from './mixins/resize'
	import china from 'echarts/map/js/china.js'

	export default {
		mixins: [resize],
		data() {
			return {
				value1: '',
				sbzk: [{
						name: '设备总数',
						value: '1556542',
						bl: '13.6'
					},
					{
						name: '本月新增',
						value: '3203',
						bl: '13.6'
					},
					{
						name: '本周新增',
						value: '124',
						bl: '13.6'
					}
				],
				tableData: [
					{
						czzh: '1',
						czip: '深圳',
						cznr: '2715',
						czsj: '85%'
					},
					{
						czzh: '2',
						czip: '深圳',
						cznr: '2715',
						czsj: '85%'
					},
					{
						czzh: '3',
						czip: '深圳',
						cznr: '2715',
						czsj: '85%'
					},
					{
						czzh: '4',
						czip: '深圳',
						cznr: '2715',
						czsj: '85%'
					},
					{
						czzh: '5',
						czip: '深圳',
						cznr: '2715',
						czsj: '85%'
					},
					{
						czzh: '6',
						czip: '深圳',
						cznr: '2715',
						czsj: '85%'
					},
					{
						czzh: '7',
						czip: '深圳',
						cznr: '2715',
						czsj: '85%'
					},
					{
						czzh: '8',
						czip: '深圳',
						cznr: '2715',
						czsj: '85%'
					},
					{
						czzh: '9',
						czip: '深圳',
						cznr: '2715',
						czsj: '85%'
					},
					{
						czzh: '10',
						czip: '深圳',
						cznr: '2715',
						czsj: '85%'
					}
				],
				chart: null,
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
				qhkeyword: ''
			}
		},
		mounted() {
			this.initChart()
		},
		methods: {
			initChart() {
				this.chart = echarts.init(document.getElementById('map'))
				this.chart.setOption({
					title: {
						// text: '全国设备分布图',
						// sublink: 'http://zh.wikipedia.org/wiki/%E9%A6%99%E6%B8%AF%E8%A1%8C%E6%94%BF%E5%8D%80%E5%8A%83#cite_note-12'
					},
					tooltip: {
						trigger: 'item',
						formatter: '{b}{c}(台)'
					},
					toolbox: {
						show: true,
						orient: 'vertical',
						left: 'right',
						top: 'center',
					},
					visualMap: {
						min: 0,
						max: 5000,
						// text: ['High', 'Low'],
						realtime: false,
						calculable: false,
						inRange: {
							color: ['lightskyblue', 'yellow', 'orangered']
						}
					},
					series: [{
						name: '',
						type: 'map',
						mapType: 'china', // 自定义扩展图表类型
						// label: {
						//     show: true
						// },
						data: [{
								name: '北京',
								value: Math.round(Math.random() * 500)
							},
							{
								name: '天津',
								value: Math.round(Math.random() * 5)
							},
							{
								name: '上海',
								value: Math.round(Math.random() * 500)
							},
							{
								name: '重庆',
								value: Math.round(Math.random() * 500)
							},
							{
								name: '河北',
								value: Math.round(Math.random() * 500)
							},
							{
								name: '河南',
								value: Math.round(Math.random() * 5000)
							},
							{
								name: '云南',
								value: Math.round(Math.random() * 500)
							},
							{
								name: '辽宁',
								value: Math.round(Math.random() * 500)
							},
							{
								name: '黑龙江',
								value: Math.round(Math.random() * 500)
							},
							{
								name: '湖南',
								value: Math.round(Math.random() * 500)
							},
							{
								name: '安徽',
								value: Math.round(Math.random() * 500)
							},
							{
								name: '山东',
								value: Math.round(Math.random() * 500)
							},
							{
								name: '新疆',
								value: Math.round(Math.random() * 500)
							},
							{
								name: '江苏',
								value: Math.round(Math.random() * 500)
							},
							{
								name: '浙江',
								value: Math.round(Math.random() * 500)
							},
							{
								name: '江西',
								value: Math.round(Math.random() * 500)
							},
							{
								name: '湖北',
								value: Math.round(Math.random() * 500)
							},
							{
								name: '广西',
								value: Math.round(Math.random() * 500)
							},
							{
								name: '甘肃',
								value: Math.round(Math.random() * 500)
							},
							{
								name: '山西',
								value: Math.round(Math.random() * 500)
							},
							{
								name: '内蒙古',
								value: Math.round(Math.random() * 500)
							},
							{
								name: '陕西',
								value: Math.round(Math.random() * 500)
							},
							{
								name: '吉林',
								value: Math.round(Math.random() * 500)
							},
							{
								name: '福建',
								value: Math.round(Math.random() * 500)
							},
							{
								name: '贵州',
								value: Math.round(Math.random() * 500)
							},
							{
								name: '广东',
								value: Math.round(Math.random() * 500)
							},
							{
								name: '青海',
								value: Math.round(Math.random() * 500)
							},
							{
								name: '西藏',
								value: Math.round(Math.random() * 500)
							},
							{
								name: '四川',
								value: Math.round(Math.random() * 500)
							},
							{
								name: '宁夏',
								value: Math.round(Math.random() * 500)
							},
							{
								name: '海南',
								value: Math.round(Math.random() * 500)
							},
							{
								name: '台湾',
								value: Math.round(Math.random() * 500)
							},
							{
								name: '香港',
								value: Math.round(Math.random() * 500)
							},
							{
								name: '澳门',
								value: Math.round(Math.random() * 500)
							},
							{
								name: '南海诸岛',
								value: Math.round(Math.random() * 500)
							}
						],
					}]
				})
			}
		}
	}
</script>
<style scoped="scoped">
	.screen {
		font-size: 0.875rem;
	}

	.select {
		width: 10rem;
	}

	.btn {
		margin-left: 0.9375rem;
	}

	.sbzk {
		line-height: 1.5;
		width: 30%;
		padding: 1%;
		margin-left: 1.5%;
		margin-top: 1.5%;
		text-align: center;
		background-color: white;
		box-shadow: 5px 3px 5px 0px rgba(100, 94, 94, 0.75);
		-webkit-box-shadow: 5px 3px 5px 0px rgba(100, 94, 94, 0.75);
		-moz-box-shadow: 5px 3px 5px 0px rgba(100, 94, 94, 0.75);
	}

	.bigtitle {
		font-size: 1.25rem;
	}
</style>
