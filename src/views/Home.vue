<template>
  <div>
    <el-container>
      <el-header>
        <h2>天气查询</h2></el-header>
      <el-main>
        <div class="main">
          <div style="float: left;">
            <el-input placeholder="请输入城市名如:北京" v-model="input"  @keyup.enter.native="getWeather()" clearable> </el-input>
          </div>
          <div style="float: right;">
            <el-button
              type="primary"
              icon="el-icon-search"
              @click="getWeather()"
              >搜索</el-button
            >
          </div>
        </div>
        <div class="card">
          <el-card v-show="cardShow">
            <ul>
              <li v-for="item in weather" :key="item.date">
                日期：{{ item.date }}温度：{{ item.low }} ~ {{ item.high }}风:{{
                  item.fengxiang + item.fengli
                }}天气：{{ item.type }}
              </li>
            </ul>
          </el-card>
        </div>
      </el-main>
    </el-container>
  </div>
</template>

<script>
export default {
  data() {
    return {
      cardShow: false,
      weather: "",
      input: "北京",
      city: "北京",
    };
  },
  methods: {
    getWeather() {
      var that = this;
      this.city = this.input;
      this.$axios
        .get(`http://wthrcdn.etouch.cn/weather_mini?city=${this.city}`)
        .then((res) => {
          console.log(res);
          console.log(res.data.desc);
          if (res.data.status == 1000) {
            that.weather = res.data.data.forecast;
            that.cardShow = true;
            that.$message({
              message: "查询成功",
              type: "success",
            });
          } else {
            that.cardShow = false;
            that.$message.error("查找失败，请核对城市名");
          }
        });
      /*.catch(err => {
          console.log(err);
        })
        .finally(() => {});*/
    },
    keyEnter(){
      this.getWeather();
    }
  },
};
</script>

<style>
.el-header {
  background-color: #00bfff;
  color: rgb(241, 239, 239);
  text-align: center;
  height: 60px;
}
.el-main {
  background-color: #f5f5f5;
  margin-top: 10px;
  height: 490px;
}
.main {
  margin-top: 10px;
  text-align: center;
  margin-left: 35%;
  margin-right: 35%;
}
.card {
  margin-top: 80px;
  text-align: left;
  margin-left: 15%;
  margin-right: 15%;
}
</style>
