<template>
  <div class="about" style="margin-top: 50px">
    <div id="mainChart" :style="{width: '100%', height: '300px'}"></div>
  </div>
</template>

<script>
   const option = {
          title: {
              text: '全国疫情趋势',
              textStyle:{
                color:'#333',
                fontSize:16
              }
          },
          tooltip: {
              trigger: 'axis'
          },
          legend: {
              data: ['现存确诊', '现存疑似']
          },
          grid: {
              left: '3%',
              right: '4%',
              bottom: '3%',
              containLabel: true
          },
          toolbox: {
              feature: {
                  saveAsImage: {}
              }
          },
          xAxis: {
              type: 'category',
              // boundaryGap: false,
              data:[]
          },
          yAxis: {
              type: 'value'
          },
          series: [
              {
                  name: '现存确诊',
                  type: 'line',
                  smooth:true,
                  data:[],
                  itemStyle: {
                    normal: {
                       color: "#e83132",//折线点的颜色
                       lineStyle: {
                       color: "#e83132"//折线的颜色
                      }
                    }    
                  }    

              },
              {
                  name: '现存疑似',
                  type: 'line',
                  smooth:true,
                  data:[],
                  itemStyle: {
                    normal: {
                       color: "#10aeb5",//折线点的颜色
                       lineStyle: {
                       color: "#10aeb5"//折线的颜色
                      }
                    }    
                  }    
              },
          ]
      };
export default {
  name: 'about',
  props:{
    historylist:{
      type:Array
    }
  },
  data () {
    return {
      // chartData:
      myChart:'',
    }
  },
  mounted(){
    this.myChart=this.$echarts.init(document.getElementById('mainChart'));

  },
  watch:{
    historylist(val,oldVal){
      if(val !==oldVal)
      this.getData(val);
    }
  },
  methods:{
  getData(historylist){
    let vm=this,date=[],seriesDateOne=[],seriesDateTwo=[];
    if(historylist){
    let list=historylist.reverse().slice(-30)
      list.forEach(data=>{
        date.push(data.date.substr(1,5));
        seriesDateOne.push(data.cn_econNum)
        seriesDateTwo.push(data.cn_susNum)
      })
      option.xAxis.data=date;
      option.series[0].data=seriesDateOne;
      option.series[1].data=seriesDateTwo;
      this.myChart.setOption(option);
    }
   },
  }
}
</script>
