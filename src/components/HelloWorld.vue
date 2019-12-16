<template>
  <div class="hello">
    <!-- <h2>{{msg}}</h2> -->
    <h1>{{ msg.data.data.city }}天气预报</h1>
    <p>时间：{{time}}</p>
    <p>现在温度:{{msg.data.data.wendu}}℃</p>
    <p>感冒指数:{{msg.data.data.ganmao}}</p>
    <div>
      查询城市：<input type="text" id="city" v-model="city" @keyup.enter="sub" />&nbsp;<input type="button" @click="sub"
        value="查询" />
    </div>
    <table border="1">
      <tr>
        <th>日期/星期</th>
        <th>最高温度</th>
        <th>风力</th>
        <th>最低温度</th>
        <th>风向</th>
        <th>天气</th>
      </tr>
      <tr>
        <td>{{msg.data.data.yesterday.date}}</td>
        <td>{{msg.data.data.yesterday.high}}</td>
        <td>{{msg.data.data.yesterday.fl}}</td>
        <td>{{msg.data.data.yesterday.low}}</td>
        <td>{{msg.data.data.yesterday.fx}}</td>
        <td>{{msg.data.data.yesterday.type}}</td>
      </tr>
      <tr v-for="ms in msg.data.data.forecast" :key="ms.date">
        <td>{{ms.date}}</td>
        <td>{{ms.high}}</td>
        <td>{{ms.fengli}}</td>
        <td>{{ms.low}}</td>
        <td>{{ms.fengxiang}}</td>
        <td>{{ms.type}}</td>
      </tr>
    </table>
    <!-- <div>
      <textarea v-model="content">

      </textarea>
      <input type="button" @click="submit" value="提交">
    </div> -->
  </div>

</template>

<script>
  import axios from "axios";
  //alert(0)
  export default {
    title: "天气预报",
    name: "HelloWorld",
    content: '',
    data() {
      return {
        msg: "",
        city: "石家庄",
        time: ""
      };
    },
    mounted() {
      axios
        .get("http://wthrcdn.etouch.cn/weather_mini?city=石家庄")
        .then(response => (this.msg = response))
        .catch(function(err) {
          console.log(err);
          alert(err);
        });
      this.datetime();
      setInterval(this.datetime, 1000);
    },
    methods: {
      submit: function() {
        alert("sub" + this.content);
        axios.post('http://localhost:8081/hi', {
            name: this.content
          })
          .then((response) => {
              alert(response)
            },
            (response) => {
              alert('error')
            })
      },
      sub: function() {
        if (this.city == "") {

        } else {
          axios
            .get("http://wthrcdn.etouch.cn/weather_mini?city=" + this.city)
            .then(response => (this.msg = response))
            .catch(function(err) {
              console.log(err);
              alert(err);
            });
        }
      },
      datetime() {
        var myDateTime = new Date();
        this.time = myDateTime.toLocaleString();
      }
    }
  };
</script>


<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
  h1,
  h2 {
    font-weight: normal;
  }

  ul {
    list-style-type: none;
    padding: 0;
  }

  li {
    display: inline-block;
    margin: 0 10px;
  }

  a {
    color: #42b983;
  }

  table {
    margin: auto;
    width: 70%
  }
</style>
