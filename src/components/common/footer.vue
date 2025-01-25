<template>
  <el-row type="flex" justify="center" class="footer">
    <el-col :span="20">
      <div>Copyright © 2024 知音多媒体在线学习平台</div>
    </el-col>
  </el-row>
</template>

<script>
export default {
  name: 'Footer'
}
</script>

<style scoped lang="scss">
.footer {
  margin-top: 36px;
  padding: 20px;
  text-align: center;
  background-color:#fb4a3e;

  * {
    color: #ffffff;
    font-size: 13px;
  }
}
</style>
