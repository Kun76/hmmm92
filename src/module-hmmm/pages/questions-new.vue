<template>
  <div class="dashboard-container">
    <div class="app-container">
      <el-card class="box-card">
        <el-form ref="addFormRef" :model="addForm" label-width="120px">
          <!--:model作用：可以使得el-form针对表单的全部信息进行收集，固定属性-->
          <el-form-item label="学科：">
            <el-select v-model="addForm.subjectID">
              <el-option
                v-for="item in subjectIDList"
                :key="item.value"
                :value="item.value"
                :label="item.label"
              ></el-option>
            </el-select>
          </el-form-item>
          <el-form-item label="目录：">
            <el-select v-model="addForm.catalogID">
              <el-option
                v-for="item in catalogIDList"
                :key="item.value"
                :label="item.label"
                :value="item.value"
              ></el-option>
            </el-select>
          </el-form-item>
          <el-form-item label="企业：">
            <el-select v-model="addForm.enterpriseID">
              <el-option
                v-for="item in enterpriseIDList"
                :key="item.id"
                :label="item.shortName"
                :value="item.id"
              ></el-option>
            </el-select>
          </el-form-item>
          <el-form-item label="城市：">
            <el-select v-model="addForm.province" @change="addForm.city=''">
              <el-option v-for="item in provinces()" :key="item" :label="item" :value="item"></el-option>
            </el-select>
            <el-select v-model="addForm.city">
              <el-option
                v-for="item in citys(addForm.province)"
                :key="item"
                :label="item"
                :value="item"
              ></el-option>
            </el-select>
          </el-form-item>
          <el-form-item label="方向：">
            <el-select v-model="addForm.direction">
              <el-option v-for="item in directionList" :key="item" :label="item" :value="item"></el-option>
            </el-select>
          </el-form-item>
        </el-form>
      </el-card>
    </div>
  </div>
</template>

<script>
import { simple as subjectsSimple } from '@/api/hmmm/subjects' // 学科
import { simple as directorysSimple } from '@/api/hmmm/directorys' // 二级目录
import { provinces, citys } from '@/api/hmmm/citys' // 城市和区县
import { direction as directionList } from '@/api/hmmm/constants' // 常量(方向)
import { list } from '@/api/hmmm/companys' // 企业
export default {
  name: 'QuestionsNew',
  data() {
    return {
      enterpriseIDList: [], // 企业
      subjectIDList: [], // 学科
      catalogIDList: [], // 二级目录
      directionList, // 方向(简易成员赋值)
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
  },
  methods: {
    provinces, // 简体成员赋值 城市
    citys, // 县区
    // 获得 企业 列表信息
    async getEnterpriseIDList() {
      var result = await list()
      this.enterpriseIDList = result.data.items
    },
    // 学科 列表信息
    async getSubjectIDList() {
      var result = await subjectsSimple()
      this.subjectIDList = result.data
    },
    // 目录 列表信息
    async getCatalogIDList() {
      var result = await directorysSimple()
      this.catalogIDList = result.data
    }
  },
  created() {
    this.getSubjectIDList() // 学科
    this.getCatalogIDList() // 二级目录
    this.getEnterpriseIDList() // 获得企业
  }
}
</script>

<style scoped>
</style>
