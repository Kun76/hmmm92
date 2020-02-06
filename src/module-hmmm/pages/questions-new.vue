<template>
  <div class="dashboard-container">
    <div class="app-container">
      <el-card class="box-card">
        <el-form ref="addFormRef" :model="addForm" label-width="120px">
          <!--:model作用：可以使得el-form针对表单的全部信息进行收集，固定属性-->
          <el-form-item label="学科：">
            <el-select v-model="addForm.subjectID"></el-select>
          </el-form-item>
          <el-form-item label="目录：">
            <el-select v-model="addForm.catalogID"></el-select>
          </el-form-item>
          <el-form-item label="企业：">
            <el-select v-model="addForm.enterpriseID"></el-select>
          </el-form-item>
          <el-form-item label="城市：">
            <el-select v-model="addForm.province" @change="getCitys(addForm.province)"></el-select>
            <el-select v-model="addForm.city"></el-select>
          </el-form-item>
          <el-form-item label="方向：">
            <el-select v-model="addForm.direction"></el-select>
          </el-form-item>
        </el-form>
      </el-card>
    </div>
  </div>
</template>

<script>
export default {
  name: 'QuestionsNew',
  data() {
    return {
      // 添加试题 表单数据对象
      addForm: {
        subjectID: '', // 学科
        catalogID: '', // 二级目录
        enterpriseID: '', // 企业
        city: '', // 区县
        province: '', // 城市
        direction: '' // 方向
      }
    }
  }
}
</script>

<style scoped>
</style>
