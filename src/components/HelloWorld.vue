<template>
  <div class="hello">
    <button @click="click">登录</button>
    <button @click="write">写入数据</button>
  </div>
</template>

<script>
import cloudbase from '@cloudbase/js-sdk';

const app = cloudbase.init({
  env: 'web-3gjrbbmi2da1e089'
});

const auth = app.auth({ persistence: 'none' });

export default {
  name: 'HelloWorld',
  props: {
    msg: String
  },
  methods: {
    async click() {
      // 1. 建议登录前检查当前是否已经登录
      if(!auth.hasLoginState()){
        // 2. 请求开发者自有服务接口获取ticket
        var url = "https://web-3gjrbbmi2da1e089-1316911075.ap-shanghai.app.tcloudbase.com/admin/login";
        let ticket = await this.axios.get(url,{
          params:{
              'username': 'admin',
              'password': 'admin'
            }
          }).then(function (res) {
            return res.data
          }).catch(function () {
            return ""
          });
        if(ticket !== "") {
          // 3. 登录 CloudBase
          await auth.customAuthProvider().signIn(ticket);
        }
        
      }
    },
    write() {
      let authHeader = cloudbase.auth().getAuthHeader();
      var url = "https://web-3gjrbbmi2da1e089-1316911075.ap-shanghai.app.tcloudbase.com/test/add";
      this.axios.get(url, {
        headers: {
          ...authHeader
        }
      }).then(function (res) {
        alert(res.data)
      }).catch(function (err) {
        alert(err)
      });
    }
  }
}

</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
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
</style>
