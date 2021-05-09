<template>
  <div class="container">
    <div class="lin-info">
      <h1>中医处方辅助推荐系统</h1>
    </div>
    <el-form :model="form" status-icon ref="form" @submit.native.prevent>
      <div>
        <span><a>姓名</a><input type="text" v-model="form.name"/></span>
        <span><a>年龄</a><input type="text" v-model="form.age"/></span>
        <span
          ><a>性别</a>
          <select v-model="form.sex">
            <option value="男">男</option>
            <option value="女">女</option>
          </select>
        </span>
      </div>
      <div class="add">
        <a>症状名</a>
        <select v-model="aaa">
          <option v-for="ss in sym" :value="ss.symptom" v-bind:key="ss">
            {{ ss.symptom }}
          </option>
        </select>
        <span><el-button @click="getSymptoms()">添加症状</el-button></span>
        <span><el-button @click="emptyList()">清空列表 </el-button></span>
        <span><el-button type="primary" @click="submitform()" :loading="loading">自动开方</el-button></span>
      </div>
      <div class="list">
        <span
          ><a>症状列表：</a>
          <input class="e-list" type="text" v-model="form.symptom" />
        </span>
      </div>
    </el-form>
    <div>
      <span>
        <a>开放报告：</a>
        <div class="r-list" v-for="item in group" v-bind:key="item">
          <div class="baogao"><a>病人开方报告</a></div>
          <div class="info">
            <a>姓名：{{ item.name }}</a>
            <a>年龄：{{ item.age }}</a>
            <a>性别：{{ item.sex }}</a>
          </div>
          <div class="state">
            <a>病人症状：{{ item.symptom }}</a>
          </div>
          <div class="result">
            <a>处方推荐结果：</a>
          </div>
          <div class="res">
            <a>{{ item.prescription }}</a>
          </div>
        </div>
      </span>
    </div>
  </div>
</template>

<!--{-->
<!--"id": 43,-->
<!--"name": "bsj",-->
<!--"age": 20,-->
<!--"sex": "man",-->
<!--"symptom": "头痛,发热,汗出",-->
<!--"prescription": "推荐的草药为：[['斑蟊|猫', '普通二分至三分。'], ['龙胆', '普通五分至钱半。'], ['铁精', '普通内服一厘至五厘，煎服者虽数钱亦可，近时多用以融化他药中服之。'], ['白头翁', '普通钱半至三钱。'], ['栀子', '普通一钱至三钱。'], ['白垩', '普通数分至一钱。'], ['云母', ' 一般内服数分起。'], ['天名精', '普通一二钱。'], ['龙骨', '普通二钱至三四钱。'], ['白瓜子', None], ['葶苈', '普通八分至二三钱。'], ['瓜蒂', '普通一钱至二钱。'], ['石灰', '普通三分至一钱。'], ['五加皮', '普通一钱至三钱。'], ['阳起石', '普通水飞用，五分至一钱。'], ['槐实', '普通一钱至三钱。']]"-->
<!--}-->

<script>
import patient from '@/model/patient'

export default {
  data() {
    return {
      form: {
        name: '',
        age: '',
        sex: '',
        symptom: '',
      },
      sym: [],
      aaa: '',
      group: [
        {
          prescription: '',
        },
      ],
    }
  },
  methods: {
    async getSymptoms() {
      try {
        this.loading = true
        this.sym = await patient.getSymptoms()
        this.form.symptom = `${this.form.symptom + this.aaa }    `
        this.loading = false
      } catch (e) {
        this.loading = false
        console.log(e)
      }
    },
    async submitform() {
      try {
        this.loading = true
        this.group = await patient.createPatient(this.form)
        this.loading = false
      } catch (e) {
        this.loading = false
        console.log(e)
      }
    },
    async emptyList() {
      this.form.symptom = ''
    },
  },
  async created() {
    this.getSymptoms()
  },
}
</script>

<style scoped lang="scss">
.container {
  padding: 20px;
  background: #d6d6d6;
  .lin-info {
    display: flex;
    flex: 1;
    height: 60px;
    width: 100%;
    h1 {
      font-family: 'myriad-pro', 'Myriad Pro', 'Helvetica Neue', Helvetica, Arial, sans-serif;
      font-size: 25px;
      position: relative;
      padding-left: 300px;
    }
  }
}
span {
  input {
    margin-left: 10px;
    font-size: 15px;
    width: 60px;
    height: 30px;
  }
  select {
    margin-left: 10px;
    width: 60px;
    height: 30px;
  }
}
.add {
  margin-top: 30px;
  select {
    margin-left: 20px;
    width: 100px;
    height: 30px;
  }
  span {
    margin-left: 50px;
  }
}
a {
  font-family: 'tinymce-mobile', sans-serif;
  font-size: 20px;
  margin-left: 120px;
}
.list {
  margin-top: 30px;
  height: 100px;
}
.e-list {
  margin-left: 220px;
  background: #ecf1ff;
  border-style: ridge;
  color: black;
  width: 60%;
  height: 60px;
  word-wrap: break-word;
  text-align: center;
  a {
    font-size: 25px;
    margin-left: 20px;
    margin-right: 50px;
  }
}
size {
  width: 120px;
}
.r-list {
  margin-left: 220px;
  background: #ecf1ff;
  border-style: ridge;
  color: black;
  width: 60%;
  height: 350px;
  .baogao {
    border-bottom: solid 1px;
    padding-top: 10px;
    padding-left: 100px;
    padding-bottom: 10px;
    height: 20px;
  }
  .info,
  .state,
  .result,
  .res {
    border-bottom: solid 1px;
    height: 30px;
    padding-top: 10px;
    padding-bottom: 10px;
    a {
      font-size: 15px;
      margin-left: 60px;
    }
  }
  .result {
    border-bottom: none;
    height: 10px;
  }
  .res {
    border-bottom: none;
    word-break: break-all;
  }
}

@media screen and (max-width: 1200px) {
  .container .lin-info .lin-info-right {
    display: none;
  }
  .container .lin-info .lin-info-left {
    width: 100%;
  }
  .container .quantity-statistics .quantity-item {
    width: 32%;
    &:last-child {
      display: none;
    }
  }
  .container .information .personal {
    display: none;
  }
}

@media screen and (max-width: 1200px) {
  .container .lin-info .lin-info-left {
    width: 100%;
  }
}
</style>
