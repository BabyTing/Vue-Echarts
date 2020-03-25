<template>
  <div class="home">
    <Map v-bind:list="dataList"></Map>
    <Chart :historylist="historylist"></Chart>
    <div class="case">
    	<p>国内各地区疫情</p>
    	<table>
    		<thead>
    			<tr>
	    			<th>地区</th>
	    			<th>现存确诊</th>
	    			<th>累计确诊</th>
	    			<th>死亡</th>
	    			<th>治愈</th>
	    		</tr>
    		</thead>
    		
    		<tbody v-for="(province,index) in tableData">
	    		<tr>
	    			<td class="province"><div :class="province.isShowDetail?'bottomIcon':'leftIcon'" v-if="province.city.length>0" @click="showDetail(index)"></div><span v-else class="spansStyle"></span>{{province.name}}</td>
	    			<td>{{province.econNum}}</td>
	    			<td>{{province.susNum}}</td>
	    			<td>{{province.deathNum}}</td>
	    			<td class="cureDataStyle">{{province.cureNum}}</td>
	    		</tr>
	    		<tr v-show="province.isShowDetail">
	    			<td colspan="5">
	    				<table>
			    			<tr v-for="city in province.city">
			    				<td class="cityName">{{city.name}}</td>
			    				<td>{{city.econNum}}</td>
				    			<td>{{city.susNum}}</td>
				    			<td>{{city.deathNum}}</td>
				    			<td>{{city.cureNum}}</td>
			    			</tr>
			    		</table>
	    			</td>
	    		</tr>
    		</tbody>
    		
    	</table>
    </div>
  </div>
</template>
<style>
	.home{
		padding: 10px 15px;
	}
	.case{
 		margin-top: 20px
	}
	.case p{
		font-weight: bold;
		padding-left: 5px;
	}
	.leftIcon{
		display: inline-block;
		border-left: 7px solid white;
		border-right: 7px solid transparent;
		border-top: 7px solid transparent;
		border-bottom: 7px solid transparent;
		cursor: pointer;
		margin-left: 10px
	}
	.bottomIcon{
		display: inline-block;
		border-left: 7px solid transparent;
		border-right: 7px solid transparent;
		border-top: 7px solid white;
		border-bottom: 7px solid transparent;
		cursor: pointer;
		margin-left: 10px;
		margin-right: 5px;
		position: relative;
		top: 3px;
	}
	.province{
		border-bottom-left-radius: 10px;
		border-top-left-radius: 10px;
		text-align: left;
		background: #10aeb5;
		font-weight: bold;

	}
	.cityName{
		color: #10aeb5;
        font-weight: bold;
	}
	.spansStyle{
		padding-left: 25px;
	}
	.cureDataStyle{
		border-bottom-right-radius: 10px;
		border-top-right-radius: 10px;
	}
	 table{
		border-collapse: separate;
		table-layout: fixed;
	    width: 100%;
	    text-align: center;
	    border-spacing: 5px 10px;
	} 

  	tbody tr{
	    background: #9999990d;
	    height: 40px;
	    font-size: 15px
	 } 
	tbody table{
		background: white;
		border-collapse: collapse;
        border-spacing:0;
	}
	tbody table tr{
		height: 45px;
		background: white;
        font-size: 14px;
		border: 1px solid #9999991c;
	} 
</style>
<script>
import Map from '@/components/Map.vue'
import Chart from '@/components/Chart.vue'
import Vue from 'vue'

export default {
  name: 'home',
  data () {
    return {
    	dataList:{},
    	historylist:[],
    	tableData:[]
    }
  },
  mounted(){
  	this.getDate();

  },
  methods:{
  	getDate(){
  		var param = {
	        header: {
	          "content-type": "text/html" 
	        }
	      };
	      this.$jsonp('https://interface.sina.cn/news/wap/fymap2020_data.d.json?_=1580892522427',param).then(res=>{
	      	this.dataList=res.data;
	      	this.historylist=res.data.historylist
	      	this.tableData=res.data.list.sort(this.compare('econNum'))
	      	this.tableData.forEach(elem=>{
  				Vue.set(elem, 'isShowDetail', false);
	      	})
	      })
  	},
  	showDetail(index){
  		this.tableData.forEach((elem, eIndex)=>{
			if(eIndex == index){
				this.tableData[index].isShowDetail = !this.tableData[index].isShowDetail;
			}else{
				this.tableData[eIndex].isShowDetail = false;
			}
		})
  	},
  	compare(prop) {
      return function(obj1, obj2) {
        var val1 = parseFloat(obj1[prop]);
        var val2 = parseFloat(obj2[prop]);
        if (val2 < val1) {
          return -1;
        } else if (val2 > val1) {
          return 1;
        } else {
          return 0;
        }
      }
    },
  	  
  },
  components:{
    Map,
    Chart
  }
}
</script>
