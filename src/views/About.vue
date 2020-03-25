<template>
  <div class="about">
    <Fmap :listData="listData"></Fmap>
    <div id="mainChart" :style="{width: '100%', height: '300px'}"></div>
        <div class="case">
      <p>国内各地区疫情</p>
      <table>
        <thead>
          <tr>
            <th>国家</th>
            <th>新增确诊</th>
            <th>现存确诊</th>
            <th>死亡</th>
            <th>治愈</th>
          </tr>
        </thead>
        
        <tbody v-for="(foreign,index) in tableData">
          <tr>
            <td class="foreign"><span class="spanStyle"></span>{{foreign.name}}</td>
            <td>{{foreign.conadd}}</td>
            <td>{{foreign.conNum}}</td>
            <td>{{foreign.deathNum}}</td>
            <td class="cureDataStyle">{{foreign.cureNum}}</td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>
</template>
<style>
  .about{
    padding: 10px 15px
  }
  #mainChart{
    padding: 15px 0;
  }
  .case p{
    font-weight: bold;
    padding-left: 5px;
  }
 .foreign{
  color: #10aeb5;
  font-weight: bold;
 }
  .spanStyle{
    padding-left: 10px;
  }
   table{
    border-collapse: separate;
    table-layout: fixed;
      width: 100%;
      text-align: center;
      border-spacing: 5px 10px;
  } 

  tbody{
      /*background: #9999990d;*/
      height: 40px;
  } 
</style>
<script>
import Fmap from '@/components/Fmap.vue'

   const option = {
          title: {
              text: '中国、海外疫情趋势'
          },
          tooltip: {
              trigger: 'axis'
          },
          legend: {
              data: ['中国新增确诊', '海外新增确诊']
          },
          grid: {
              left: '3%',
              right: '4%',
              bottom: '3%',
              containLabel: true
          },
          xAxis: {
              type: 'category',
              // boundaryGap: false,
              data:[],
          },
          yAxis: {
              type: 'value'
          },
          series: [
              {
                  name: '中国新增确诊',
                  type: 'line',
                  smooth:true,
                  data:[],
                  symbol:'circle', 
                  itemStyle: {
                    normal: {
                       color: "#ff6a57",//折线点的颜色
                       lineStyle: {
                       color: "#ff6a57"//折线的颜色
                      }
                    }    
                  }    
              },
              {
                  name: '海外新增确诊',
                  type: 'line',
                  smooth:true,
                  data:[],
                  symbol:'circle', 
                  itemStyle: {
                    normal: {
                       color: "#ec9217",//折线点的颜色
                       lineStyle: {
                       color: "#ec9217"//折线的颜色
                      }
                    }    
                  }            
              },
          ]
      };
export default {
  name: 'about',
  data () {
    return {
      myChart:'',
      listData:{},
      tableData:[],
    }
  },
  mounted(){
    this.getData();
    this.myChart=this.$echarts.init(document.getElementById('mainChart'));

  },
 methods:{
  getData(){
      var param = {
        header: {
          "content-type": "text/html" 
        }
      };
      this.$jsonp('https://interface.sina.cn/news/wap/fymap2020_data.d.json?_=1580892522427',param).then(res=>{
        console.log(res)
        let date=[],seriesDateOne=[],seriesDateTwo=[];
        let otherhistorylist=res.data.otherhistorylist.reverse().slice(-30)
        otherhistorylist.forEach(data=>{
          date.push(data.date.substr(1,5));
          seriesDateTwo.push(data.certain_inc)
        })
        res.data.historylist.forEach(data=>{
          seriesDateOne.push(data.cn_conadd)
        })
        option.xAxis.data=date;
        option.series[0].data=seriesDateOne;
        option.series[1].data=seriesDateTwo;
        this.myChart.setOption(option);
        
        this.listData=res.data;

        let compare=function (property){
          return function(a,b){
              var value1 = a[property];
              var value2 = b[property];
              return value1 - value2;
          }
      }
        this.tableData=res.data.otherlist.sort(this.compare('conadd'))
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
    Fmap,
  }
}
</script>
