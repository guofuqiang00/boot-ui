<template>
  <div class="father">
    <el-divider>父組件</el-divider>
    <el-form :model="ruleForm" status-icon :rules="rules" ref="ruleForm" label-width="100px" class="demo-ruleForm">
      <el-form-item label="密码" prop="pass">
        <el-input type="text" v-model="ruleForm.pass" autocomplete="off"></el-input>
      </el-form-item>
      <el-form-item label="确认密码" prop="checkPass">
        <el-input type="text" v-model="ruleForm.checkPass" autocomplete="off"></el-input>
      </el-form-item>
      <el-form-item>
        <el-button type="primary" @click="submitForm('ruleForm')">提交</el-button>
        <el-button @click="resetForm('ruleForm')">重置</el-button>
      </el-form-item>
    </el-form>

    <!--子组件传过来的值-->
    <h1>接收子组件传过来的值: <span style="color: #1ab394">{{message}}</span></h1>

    <el-divider>子組件</el-divider>
    <!-- 调用子组件 -->
    <!-- 当父组件数据填写完成之后，点击提交，调用子组件 -->
    <div v-if="show">

      <!-- 1.静态传递 -->
      <!--<Child password='静态传递 更改成功'></Child> --> <!-- 通过自定义属性传递数据 -->

      <!-- 2.动态传递 -->
      <!--<Child :password='this.ruleForm.checkPass'></Child>-->
      <!--<Child v-bind:password='this.ruleForm.checkPass'></Child>-->

      <!-- 3.动态传递 -->
      <Child :password='this.ruleForm.checkPass' :callback="callback"  @getChild="getSuccess"></Child>
      <Car
    </div>
  </div>
</template>
<script>
    // 引入子组件
    import Child from '../parentChild/child';
    export default {
        // 引入子组件
        components: {
            Child
        },
        data() {
            // 对表单提交，进行简单的校验，可忽略
            let validatePass = (rule, value, callback) => {
                if (value === '') {
                    callback(new Error('请输入密码'));
                } else {
                    if (this.ruleForm.checkPass !== '') {
                        this.$refs.ruleForm.validateField('checkPass');
                    }
                    callback();
                }
            };
            let validatePass2 = (rule, value, callback) => {
                if (value === '') {
                    callback(new Error('请再次输入密码'));
                } else if (value !== this.ruleForm.pass) {
                    callback(new Error('两次输入密码不一致!'));
                } else {
                    callback();
                }
            };
            // 对表单提交，进行简单的校验，可忽略

            return {
                // 表单中绑定的数据
                ruleForm: {
                    pass: '',
                    checkPass: '',
                    age: ''
                },

                // 对表单提交，进行简单的校验，可忽略
                rules: {
                    pass: [
                        { validator: validatePass, trigger: 'blur' }
                    ],
                    checkPass: [
                        { validator: validatePass2, trigger: 'blur' }
                    ]
                },
                // 对表单提交，进行简单的校验，可忽略

                show: true,
                message:"",
            };
        },
        created(){
            this.$on('getValue',(status) => {
                this.$message.success(status)
            })
        },
        methods: {

            callback(val){
                this.$message.success("父组件向子组件传递的方法==>>>"+val)
            },
            get(){
                this.$message.success("我是父组件的方法")
            },

            // 调用方法,接收子组件的传值
            getSuccess (value) {
                value.getMsg("asdsad")
                console.info("触发了哦！！！",value)
               this.message = value
                if(value){
                   //清空一 this.ruleForm = {}
                  this.$refs.ruleForm.resetFields() //清空二
                }
            },

            // 点击提交，调用方法
            submitForm(formName) {

                // 对表单提交，进行简单的校验，可忽略
                this.$refs[formName].validate((valid) => {
                    // 对表单提交，进行简单的校验，可忽略
                    if (valid) {
                        // 当校验成功时，显示子组件
                        // console.log('submit!');
                        this.show = true;
                    } else {
                        console.log('error submit!!');
                        return false;
                    }
                });
            },
            resetForm(formName) {
                this.$refs[formName].resetFields();
            }
        }
    }
</script>

