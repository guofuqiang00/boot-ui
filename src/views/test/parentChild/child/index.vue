<template>
  <div class="child">
    <el-alert
      title="新密码:"
      type="success">
      <span>{{password}}</span>
      <!-- 可直接使用从父组件接收的数据 -->
    </el-alert>
  </div>
</template>
<script>
    export default {
        data() {
            return {
                show: false,
            };
        },
        props: {
            password: String
            // 声明一个自定义的属性
        },
    }
</script>

