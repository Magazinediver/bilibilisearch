<template>
  <div class="body-sign">
    <div class="center-sign">
      <a href="/" class="logo pull-left">
        <img src="~assets/images/logo.png" height="54" alt="Porto Admin" />
      </a>

      <div class="panel panel-sign">
        <div class="panel-title-sign mt-xl text-right">
          <h2 class="title text-uppercase text-bold m-none"><i class="fa fa-user mr-xs"></i> Sign Up</h2>
        </div>
        <div class="panel-body">
          <el-form
            :model="addUserForm"
            ref="addUserFormRef"
            :rules="addUserFormRules"
          >
            <el-form-item  class="form-group mb-lg" prop="username">
              <label>Username / 用户昵称</label>
              <el-input v-model="addUserForm.username"></el-input>
            </el-form-item>

            <el-form-item class="form-group mb-lg" prop="email">
              <label>E-mail Address / 电子邮件</label>
              <el-input v-model="addUserForm.email"></el-input>
            </el-form-item>

            <div class="form-group mb-none">
              <div style="margin-left: 0" class="row">
                <el-form-item  class="col-sm-6 mb-lg" prop="password">
                  <label>Password / 密码</label>
                  <el-input type="password" v-model="addUserForm.password"></el-input>
                </el-form-item>
                <el-form-item class="col-sm-6 mb-lg" prop="mobile">
                  <label>Mobile / 手机号码</label>
                  <el-input v-model="addUserForm.mobile"></el-input>
                </el-form-item>
              </div>
            </div>

            <div  style="margin-left: 0" class="row">
              <div class="col-sm-8">
                <div class="checkbox-custom checkbox-default">
                  <input id="AgreeTerms" name="agreeterms" type="checkbox"/>
                  <label for="AgreeTerms">我同意 <a href="#">用户使用协议</a></label>
                </div>
              </div>
              <div style="margin-left: 68px" class="col-sm-4 text-right">
                <el-button type="primary" @click="addUser" class="btn btn-primary hidden-xs">确 定</el-button>
                <el-button type="primary" @click="addUser" class="btn btn-primary btn-block btn-lg visible-xs mt-lg">确 定</el-button>
              </div>
            </div>

            <span class="mt-lg mb-lg line-thru text-center text-uppercase">
								<span>or</span>
							</span>

            <div class="mb-xs text-center">
            </div>

            <p class="text-center">Already have an account?/已有帐号？<a href="javascript:void(0);" @click="postOpenPlatform()"> Sign In!/登录！</a></p>

          </el-form>
        </div>
      </div>

      <p class="text-center text-muted mt-md mb-md">&copy; Copyright 2020. All rights reserved. </p>
    </div>
  </div>

</template>

<script>
  export default {
    name: "signup",
    data(){
      // 自定义邮箱规则
      var checkEmail = (rule, value, callback) => {
        const regEmail = /^\w+@\w+(\.\w+)+$/
        if (regEmail.test(value)) {
          // 合法邮箱
          return callback()
        }
        callback(new Error('请输入合法邮箱'))
      }
      // 自定义手机号规则
      var checkMobile = (rule, value, callback) => {
        const regMobile = /^1[34578]\d{9}$/
        if (regMobile.test(value)) {
          return callback()
        }
        // 返回一个错误提示
        callback(new Error('请输入合法的手机号码'))
      }
      return {
        addUserForm: {
          username: '',
          password: '',
          email: '',
          mobile: '',
        },
        userlist: [],
        // 用户添加表单验证规则
        addUserFormRules: {
          username: [
            { required: true, message: '请输入用户名', trigger: 'blur' },
            {
              min: 2,
              max: 10,
              message: '用户名的长度在2～10个字',
              trigger: 'blur'
            }
          ],
          password: [
            { required: true, message: '请输入用户密码', trigger: 'blur' },
            {
              min: 6,
              max: 18,
              message: '用户密码的长度在6～18个字',
              trigger: 'blur'
            }
          ],
          email: [
            { required: true, message: '请输入邮箱', trigger: 'blur' },
            { validator: checkEmail, trigger: 'blur' }
          ],
          mobile: [
            { required: true, message: '请输入手机号码', trigger: 'blur' },
            { validator: checkMobile, trigger: 'blur' }
          ]
        },
      }
    },
    methods:{
      postOpenPlatform(){
        this.$router.replace('/login')
      },
      addUser () {
        this.$refs.addUserFormRef.validate(async valid => {
          // console.log(valid)
          if (!valid){
            return this.$message.error('填写的内容非法！')
            return false
          }
          const {data: res} = await this.$http.post('/bilibili/user/register/', this.addUserForm)
          if (res.meta.status !== 200) {
            return this.$message.error('注册用户失败！')
          }
          this.$message.success('注册成功')
          this.$router.replace('/login')
        })
      },

    }
  }
</script>

<style scoped>

</style>
