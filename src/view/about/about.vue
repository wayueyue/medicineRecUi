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
        <select v-model="form.symptom"
          ><option>1</option
          ><option>2</option></select
        >
        <span><el-button type="primary" @click="submitform('form')" :loading="loading">添加症状</el-button></span>
        <span><el-button @click="resetform('form')">清空列表 </el-button></span>
        <span><el-button>自动开方</el-button></span>
      </div>
      <div class="list">
        <span
          ><a>症状列表：</a>
          <div class="e-list">
            <a>xxxxx</a>
          </div></span
        >
      </div>
    </el-form>
    <div>
      <span
        ><a>开放报告：</a>
        <div class="r-list">
          <div class="baogao"><a>病人开方报告</a></div>
          <div class="info">
            <a>姓名：xxx</a>
            <a>年龄：xx</a>
            <a>性别：x</a>
          </div>
          <div class="state">
            <a>病人症状：</a>
          </div>
          <div class="result">
            <a>处方推荐结果：</a>
          </div>
          <div class="res">
            <a>xxxxx</a>
          </div>
        </div>
      </span>
    </div>
  </div>
</template>

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
      loading: false,
    }
  },
  methods: {
    async submitform(formName) {
      try {
        this.loading = true
        const res = await patient.createPatient(this.form)
        this.loading = false
        if (res.code < window.MAX_SUCCESS_CODE) {
          this.$message.success(`${res.message}`)
          this.resetForm(formName)
        }
      } catch (error) {
        this.loading = false
        this.$message.error('症状添加失败，请检测填写信息')
        console.log(error)
      }
    },
    // 重置表单
    resetform(formName) {
      this.$refs[formName].resetFields()
    },
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
  a {
    font-size: 25px;
    margin-left: 20px;
    margin-right: 50px;
  }
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
