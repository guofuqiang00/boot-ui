<template>
  <div class="child">
    <el-alert
      title="新密码:"
      type="success">
      <span>{{password}}</span>
      <!-- 可直接使用从父组件接收的数据 -->
    </el-alert>

    <!--子传父-->
   <el-row>
     <el-col :span="4"><el-input v-model="message" placeholder="请给父组件传值" /></el-col>
     <el-col :span="6"> <el-button @click="handleClick">点击给父组件传值111</el-button></el-col>
     <el-col :span="6"> <el-button @click="handleClick2">点击给父组件传值222</el-button></el-col>
     <el-col :span="3"> <el-button @click="handleClear">清空父組件数据</el-button></el-col>
     <el-col :span="3"> <el-button @click="handleParent">调用父组件的方法</el-button></el-col>
     <el-col :span="3"> <el-button @click="handleMethod">给父组件传递方法</el-button></el-col>
   </el-row>
  </div>
</template>
<script>
    export default {
        data() {
            return {
                show: false,
                message:""
            };
        },
        props: ["password","callback"],
        created(){
            this.$emit('getChild',this.message)
            this.callback("我是子组件赋值给父组件的")
        },
        watch:{
            message(val){
                this.$emit('getChild',val)
            }
        },
        methods:{

            /**给父组件传值11111**/
            handleClick(){
             //向父组件传值
              this.$emit('getChild',this.message)
            },
            /**给父组件传值222222*/
            handleClick2(){
                //向父组件传值
                this.$parent.$emit('getValue',"子组件传过来的哦")
            },
            /**清空父组件表单**/
            handleClear(){
                this.$emit('getChild',true )
            },
            /**调用父组件的方法**/
            handleParent(){
                this.$parent.get() //调用父组件的方法
            },
            /**给父组件传递方法**/
            handleMethod(){
                let val= ""
                this.$emit("getChild",this.getMsg)
            },
            getMsg(val){
               console.info(val)
            }

        }
    }
</script>

