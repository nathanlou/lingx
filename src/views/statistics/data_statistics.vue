<template>
	<div style="width: 95%;margin-left: 2.5%;">
		<div style="display: flex;width: 80%;">
			<div class="head_con" style="display: flex;" v-for="(item,index) in head_arr" :key='index'>
				<div class="cp_img">
					<img :src="item.img" />
				</div>
				<div>
					<div>{{item.name}}</div>
					<div>{{item.value}}台</div>
				</div>
			</div>
		</div>
		<chart height="100%" width="100%" style="margin-top: 1.5%;" />
		<div  class="enterpriseCard">
		 <el-card class="box-card" v-for=" i in 10 " :key='i'>
		    <div style="display: flex;justify-content: space-between;">
		       <el-button round size="small"
		       style="background-color: #55C2DD;color: #FFFFFF;" >正式企业&nbsp;|&nbsp;启用</el-button>
		       <div class="div_number_bg">{{i}}</div>
		    </div>
		     <div class="card_text">山东领先集团</div>
		     <div class="card_text"><span>负责人：</span><span>周一</span></div>
		     <div class="card_text"><span>电&nbsp;话:</span><span>13462268043</span></div>
		     <div class="card_text">地址：山东省东营市高新技术产业聚集区</div>
		     <div class="card_text">创建时间：2020-11-5</div>
		     <div style="background-color: #F0F2F5;height: 1px;margin: 3px 3px 3px 3px;" ></div>
		     <div class="card_text">总设备：522台</div>
		 </el-card>
		</div>
	</div>
</template>

<script>
	import Chart from '@/components/Charts/MixCharttwo'
	export default {
		components: { Chart },
		data() {
			return {
				head_arr:[
					{
						img:require('../../assets/img/cp.png'),
						name:'云平台控制仪',
						value:'1526'
					},
					{
						img:require('../../assets/img/cp.png'),
						name:'砂石分离机',
						value:'1526'
					},
					{
						img:require('../../assets/img/cp.png'),
						name:'洗车机',
						value:'1526'
					},
					{
						img:require('../../assets/img/cp.png'),
						name:'雾炮车',
						value:'1526'
					}
				]
			}
		},
		created: function() {
			
		},
		methods: {
			
		}
	};
</script>

<style scoped="scoped">
	.head_con{
		width: 25%;
		height: 3.125rem;
		line-height: 2;
		text-align: center;
	}
	.cp_img{
		width: 3.125rem;
		height: 3.125rem;
	}
	.cp_img img{
		width: 100%;
	}
	.enterpriseCard{
	   margin:10px 20px 20px 60px;
	   display: flex;
	   flex-wrap: wrap;
	}
	.div_number_bg{
	  background-image: url(../../assets/img/number_bg.png);
	  background-repeat: no-repeat;
	  background-size: auto;
	  width: 15px;
	  height: 25px;
	  text-align: center;
	  font-size: 12px;
	  color: #FFFFFF;
	}
	.box-card{
	  width: 18%;
	  color: gray;
	  margin: 8px;
	}
	.card_text{
	  padding: 5px 0px 5px 12px ;
	}
	@media screen and (max-width: 1024px) {	
	   .enterpriseCard{
	      margin:5px 20px 20px 20px;
	      display: flex;
	      flex-wrap: wrap;
	   }
	    .box-card {
	      width: 30%;
	      color: gray;
	      margin: 8px;
	
	    }
	    .card_text{
	      padding: 5px 0px 5px 12px ;
	    }
	    .div_number_bg{
	      background-image: url(../../assets/img/number_bg.png);
	      background-repeat: no-repeat;
	      background-size: auto;
	      width: 15px;
	      height: 25px;
	      text-align: center;
	      font-size: 12px;
	      color: #FFFFFF;
	    }
	}
</style>
