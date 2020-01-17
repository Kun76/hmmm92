<template>
  <el-row>
    <el-col :span="6">
      学科：
      <el-select v-model="searchForm.subjectID">
        <el-option
          v-for="item in subjectIDList"
          :key="item.value"
          :value="item.value"
          :label="item.label"
        ></el-option>
      </el-select>
    </el-col>
    <el-col :span="6">
      难度：
      <el-select v-model="searchForm.difficulty">
        <el-option
          v-for="item in difficultyList"
          :key="item.value"
          :value="item.value"
          :label="item.label"
        ></el-option>
      </el-select>
    </el-col>
    <el-col :span="6">
      试题类型：
      <el-select v-model="searchForm.questionType" style="width:140px;">
        <el-option
          v-for="item in questionTypeList"
          :key="item.value"
          :value="item.value"
          :label="item.label"
        ></el-option>
      </el-select>
    </el-col>
  </el-row>
</template>

<script>
// 导入获得学科简单列表的api方法
import { simple } from '@/api/hmmm/subjects' // 学科
// as给导入的成员设置别名,难度试题类型
import {
  difficulty as difficultyList, 
  questionType as questionTypeList} 
  from '@/api/hmmm/constants' // 常量数据
export default {
  name: 'QuestionsList',
  data() {
    return {
      subjectIDList: [], // 学科简单列表收集
      difficultyList, // 简易成员赋值(difficultyList:difficultyList)
      questionTypeList, // 简易成员赋值
      // 搜索题库的表单数据对象
      searchForm: {
        subjectID: '', // 学科ID
        difficulty: '',
        questionType: ''
      }
    }
  },
  methods: {
    // 获得 学科 下拉列表数据
    async getSubjectIDList() {
      var rst = await simple()
      // console.log(rst)
      // 把获得到的学科信息赋予给 subjectIDList 成员
      this.subjectIDList = rst.data
    }
  },
  created() {
    this.getSubjectIDList()
  }
}
</script>

<style scoped>
</style>
