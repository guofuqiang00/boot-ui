<template>
    <div>

      <div style="width: 80%;height:80%;margin: 60px auto;border: 1px solid">
        <div style="margin: 25px;">
          <el-steps :active="active" finish-status="success">
            <el-step title="配置赛事"></el-step>
            <el-step title="配置项目"></el-step>
            <el-step title="配置组别/小项"></el-step>
            <el-step title="核对信息"></el-step>
            <el-step title="完成"></el-step>
          </el-steps>

          <div style="display: block;width: 90%;height: 80%;margin: 20px auto;border: 1px solid">
            <div class="context" v-if="active===0">AAAA</div>
            <div class="context" v-if="active===1">BBBB</div>
            <div class="context" v-if="active===2">
              <el-form ref="form"  :model="form" :rules="rules" label-width="80px" style="margin: 10px auto;">

                <div style="border: 1px solid  slategrey;margin: 10px 10px" v-for="(item,index) in dynamicValidateForm.domains">
                <el-row :span="24" style="margin: 5px">
                  <el-col :span="6">
                    <el-form-item label="组别名称" prop="groupName">
                      <el-input v-model="item.groupName" :disabled="item.disabled"></el-input>
                    </el-form-item>
                  </el-col>
                  <el-col :span="6">
                    <el-form-item label="性别组" prop="gender">
                      <el-select v-model="item.gender" placeholder="请选择性别组" style="width: 100%" clearable size="small">
                        <el-option v-for="item in genderList" :key="item.value" :label="item.label" :value="item.value"/>
                      </el-select>
                    </el-form-item>
                  </el-col>
                  <el-col :span="3">
                    <el-form-item label-width="0px" label="" prop="name">
                      <el-button @click="deleteHandle(item)">删除</el-button>
                    </el-form-item>
                  </el-col>
                  <el-col :span="3">
                    <el-form-item label-width="0px" label="" >
                      <el-button @click="addHandle(item)">添加组别</el-button>
                    </el-form-item>
                  </el-col>
                  <el-col :span="6">
                    <el-form-item label-width="0px" label="" >
                      <el-button @click="addMin(item)">添加小项</el-button>
                    </el-form-item>
                  </el-col>
                </el-row>
                <el-row :span="24" v-for="(min,index) in item.minList">
                  <el-col :span="6">
                    <el-form-item style="text-align: left;padding-left: 5px" label="组别名称" prop="name">
                      <span>{{item.groupName}}</span>
                    </el-form-item>
                  </el-col>
                  <el-col :span="6">
                    <el-form-item label="小项名称" prop="name">
                      <el-input v-model="min.itemMinName"></el-input>
                    </el-form-item>
                  </el-col>
                  <el-col :span="6" >
                    <el-form-item label="小项类型" prop="propertyId">
                      <el-select v-model="min.propertyId" placeholder="选择小项类型" clearable size="small">
                        <el-option v-for="item in minTypeList" :key="item.value" :label="item.label" :value="item.value"/>
                      </el-select>
                    </el-form-item>
                  </el-col>
                  <el-col :span="3">
                    <el-form-item label-width="0px" label="" prop="name">
                      <el-button @click="deleteMinHandle(item,min)">删除</el-button>
                    </el-form-item>
                  </el-col>
                </el-row>
                </div>

              </el-form>

            </div>
            <div class="context" v-if="active===3">表单确认</div>
            <div class="context" style="font-weight: bolder;color: #13ce66" v-if="active===5">完 成</div>
          </div>

          <div>
            <center>
              <el-button type="primary" style="margin-top: 12px;" @click="prev" v-if="active==1||active==2||active==3">上一步</el-button>
              <el-button type="primary" style="margin-top: 12px;" @click="next" v-if="active==0||active==1||active==2">下一步</el-button>
              <el-button type="primary" style="margin-top: 12px;" @click="submitForm" v-if="active==3">提 交</el-button>
              <el-button type="primary" style="margin-top: 12px;" @click="exitForm" v-if="active==5">退 出</el-button>
            </center>
          </div>
        </div>
      </div>

    </div>
</template>

<script>
    export default {
        name: "steps",
        components:{},
        data(){
            return{
              active: 0,
              form:{},
              // 表单校验
              rules: {},
              dynamicValidateForm:{
                  domains: [
                      {
                      value: '',
                      minList:[],
                     }
                  ]
              },
              genderList: [
                  {value: '1', label: "男子"},
                  {value: '2', label: "女子"},
                  {value: '3', label: "混合"},
                  {value: '4', label: "其他"},
                ],
              /**小项类型**/
              minTypeList:[
                  {value: '1', label: "单项"},
                  {value: '2', label: "团队"},
                  {value: '3', label: "团体"},
                ],
            }
        },
        created(){

        },
        methods:{
            submitTest(){
                console.info(this.dynamicValidateForm)
            },
            /**新增組別**/
            addHandle(){
                this.dynamicValidateForm.domains.push({
                    value:"222",
                    minList:[],
                    key:Date.now()
                })
            },
            /**新增小项**/
            addMin(row){
                let minList = this.dynamicValidateForm.domains.forEach(e=>{
                   if(e.groupName==row.groupName){
                       e.disabled = true
                   }
                })
                let index =  this.dynamicValidateForm.domains.indexOf(row)
                if(index!=-1){
                    this.dynamicValidateForm.domains[index].minList.push({
                        value: "小项001",
                        key: Date.now()
                    })
                }
            },
            /**删除组别**/
            deleteHandle(row){
               let index =  this.dynamicValidateForm.domains.indexOf(row)
                if(index!=-1&&index!=0){
                    this.dynamicValidateForm.domains.splice(index, 1)
                }
            },
            /**删除小项**/
            deleteMinHandle(group,min){
                let index =  this.dynamicValidateForm.domains.indexOf(group);
                let minIndex = this.dynamicValidateForm.domains[index].minList.indexOf(min);
                if(minIndex!=-1){
                    this.dynamicValidateForm.domains[index].minList.splice(minIndex,1)
                }
                console.info("group==>",group)
                console.info("min==>",min)
            },
            /**上一步**/
            prev(){
                --this.active;
                if(this.active < 0){
                    this.active = 0
                }
            },
            /**下一步**/
            next(){
                /*++this.active;*/
                if(this.active++ > 5){
                   this.active = 0
                }
                console.info(this.active)

            },
            /**提交按钮**/
            submitForm(){
                console.info("提交表单")
                this.$message.success("提交成功")
                this.active = 5
            },
            /**退出表单**/
            exitForm(){
                history.back() //退出
                this.$router.go(-1); //后退1步
                this.$router.go(-2); //后退2补
                this.$router.go(-3); //后退3补
            }
        }
    }
</script>

<style scoped>
  .context{
    display: block;
    width: 100%;
    height: 400px;
    text-align: center;
    line-height: 400px;
  }
</style>
