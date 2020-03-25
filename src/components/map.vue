<template>
  <div class="content">
  	<div class="mydate">
  		<p>数据更新{{tableData.times}}</p>
  		<div class="virus">
  			<div class="virusdata">
  				<div class="one">现存确诊</div>
  				<div class="two" style="color: #ff6a57">{{tableData.econNum}}</div>
  				<div class="three">
  					昨日
  					<span style="color: #ff6a57">{{tableData.addecon_new}}</span>
  				</div>
  			</div>
  			<div class="virusdata">
  				<div class="one">现存重症</div>
  				<div class="two" style="color: #ec9217">{{tableData.heconNum}}</div>
  				<div class="three">
  					昨日
  					<span style="color: #ec9217">{{tableData.addhecon_new}}</span>
  				</div>
  			</div>
  			<div class="virusdata">
  				<div class="one">现存疑似</div>
  				<div class="two" style="color:#476da0">{{tableData.sustotal}}</div>
  				<div class="three">
  					昨日
  					<span style="color:#476da0">{{tableData.wjw_addsus_new}}</span>
  				</div>
  			</div>
  		</div>
  		<div class="virus" style="margin-top: 20px;">
  			<div class="virusdata">
  				<div class="one">累计确诊</div>
  				<div class="two" style="color: #e83132">{{tableData.gntotal}}</div>
  				<div class="three">
  					昨日
  					<span style="color: #e83132">{{tableData.addcon_new}}</span>
  				</div>
  			</div>
  			<div class="virusdata">
  				<div class="one">累计死亡</div>
  				<div class="two" style="color: #10aeb5">{{tableData.deathtotal}}</div>
  				<div class="three">
  					昨日
  					<span style="color: #10aeb5">{{tableData.adddeath_new}}</span>
  				</div>
  			</div>
  			<div class="virusdata">
  				<div class="one">累计治愈</div>
  				<div class="two" style="color:#4d5054">{{tableData.curetotal}}</div>
  				<div class="three">
  					昨日
  					<span style="color:#4d5054">{{tableData.addcure_new}}</span>
  				</div>
  			</div>
  		</div>
  	</div>
    <div ref="mapbox" id="myMap" style="width: 100%;height: 500px;margin:0 auto"></div>
  </div>
</template>
<style>
  .mydate{
  	padding: 0px 0 40px 0
  }
	.mydate p{
		color: #666;
		font-size: 13px;
		padding: 5px 0 15px 1%;
	}
	.virus{
		display: flex;
		flex-wrap: nowrap;
	}
	.virusdata{
		flex: 1 1 30%;
		text-align: center;
	}
	.one{
	    font-size: .9375rem;
	    height: .9375rem;
	    line-height: .9375rem;
	    margin-bottom: .625rem;
	    color: #333;
	    position: relative;
	}
	.two{
		font-size: 1.3125rem;
	    height: 1.3125rem;
	    line-height: 1.3125rem;
	    font-family: PingFangSC-Medium;
	    font-weight: 700;
	}
	.three{
		font-size: .6875rem;
	    height: .6875rem;
	    line-height: .6875rem;
	    margin-top: .375rem;
	    color: #999;
	}
</style>
<script>
import 'echarts/map/js/china'

const option = {
          title: {
              text: '中国现存确诊疫情地图',
              x:'left',
              textStyle:{
              	color:'#333',
              	fontSize:16
              }
          },
          tooltip: {
              trigger: 'item',
              // formatter:'地区：{b}<br/>确诊：{c}'
              formatter:function(params){
                    return '现存：'+params.data.econNum+'<br/>'+'死亡：'+params.data.deathNum+'</br>'+'治愈：'+params.data.susNum
               }
          },
          visualMap: [{
	          type: "piecewise",
	          show:true,
	          pieces:[
	          	{min:10000},
	          	{min:1000,max:9999},
	          	{min:100,max:999},
	          	{min:10,max:99},
	          	{min:1,max:9},
	          	{value:0}
	          ],
	          inRange: {
	            color: ["#fff", "#ffaa85", "#660208"]
	          },
	          itemWidth:10,
	          itemHeight:10
	       }],
          series: [{
             type:'map',
             map:'china',
             label:{
             	show:true,
             	color:'black',
             	fontSize:10
             },
             itemStyle:{
             	borderColor:'#999',
             	borderWidth:1
             },
             zoom:1.3,
             emphasis:{
             	label:{
             		color:'#fff',
             		fontSize:12
             	},
             	itemStyle:{
             		areaColor:'orange',
             		borderColor:'#999'
             	}
             }
          
          }]
      };
export default {
  name:'Map',
  props:{
  	list:{
  		type:Object
  	}
  },
  data () {
    return {
    	myChart:'',
    	tableData:{}
    }
  },
  mounted(){
    this.myChart=this.$echarts.init(document.getElementById('myMap'));
  },
  watch:{
  	list(val,oldVal){
  		if(val !==oldVal){
  			this.getData(val)
  		}
  	}
  },
  methods:{
  getData(dataList){
  	let vm=this;
  	vm.tableData={
		times:dataList.times,
		econNum:dataList.econNum,
		heconNum:dataList.heconNum,
		sustotal:dataList.sustotal,
		gntotal:dataList.gntotal,
		deathtotal:dataList.deathtotal,
		curetotal:dataList.curetotal,
		addecon_new:dataList.add_daily.addecon_new,
		addhecon_new:dataList.add_daily.addhecon_new,
		wjw_addsus_new:dataList.add_daily.wjw_addsus_new,
		addcon_new:dataList.add_daily.addcon_new,
		adddeath_new:dataList.add_daily.adddeath_new,
		addcure_new:dataList.add_daily.addcure_new,
	}
  	let list=dataList.list.map(item=>({
			name:item.name,
			value:item.value,
			susNum:item.susNum,
			econNum:item.econNum,
			deathNum:item.deathNum
	  }))	
  	option.series[0].data=list;
    this.myChart.setOption(option);
    window.onresize = this.myChart.resize
  }
 }
}
</script>
