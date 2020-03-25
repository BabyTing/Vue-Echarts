<template>
  <div id="app">
    <div class="content">
      <header>
        <h3>新型冠状病毒肺炎</h3>
        <h1>疫情实时大数据报告</h1>
      </header>
      <div class="list">
        <div class="nav">
          <router-link to="/">国内疫情</router-link>
          <router-link to="/about">国际疫情</router-link>
        </div>          
        <div>
          <router-view/>
        </div>
      </div>
      
    </div>
    
  </div>
</template>

<script>
export default {
  name: 'App'
}
</script>

<style>
*{
  margin: 0;
  padding: 0
}
#app{
  background: #10aeb5;

}
.content{
  max-width: 768px;
  min-height: 31.25rem;
  margin: 0 auto;
  overflow: hidden;
  box-sizing: border-box;
  line-height: normal;  

}
header{
  margin-top: 20px;
  padding-left: 30px;
  height: 80px;

/*   background-image:url(./assets/bg-header.jpg);
background-size: 695px 189px;
 background-position: 50% 50%;
 background-repeat: no-repeat; */
}
header h3,h1{
  color: white;
  line-height: 2rem
}
.list{
  /*margin-top: 60px;*/
  width: 95%;
  margin: 0 auto;
  background: white;
  border-radius: 20px;
  overflow: visible;
}
.nav{
  padding: 10px 0;
  border-bottom: 1px solid #9999991c;
  position:sticky;
  top: 0;
}
.nav a{
  text-decoration: none;
  font-size: 20px;
  font-weight: bold;
  color:#10aeb5;
  text-align: center;
  width: 49%;
  display: inline-block;
}
</style>
