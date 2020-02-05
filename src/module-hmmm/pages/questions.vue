<template>
  <div class="dashboard-container">
    <div class="app-container">
      <el-card class="box-card">
        <!-- :gutter 给各个col设置间歇，单位是像素 -->
        <el-row :gutter="20">
          <el-col :span="6">
            学科:
            <el-select v-model="searchForm.subjectID" class="wh">
              <el-option
                v-for="item in subjectIDList"
                :key="item.value"
                :value="item.value"
                :label="item.label"
              ></el-option>
            </el-select>
          </el-col>
          <el-col :span="6">
            难度:
             <el-select v-model="searchForm.difficulty" class="wh">
              <el-option
                v-for="item in difficultyList"
                :key="item.value"
                :value="item.value"
                :label="item.label"
              ></el-option>
            </el-select>
          </el-col>
          <el-col :span="6">
            试题类型:
            <el-select v-model="searchForm.questionType" class="wh">
              <el-option
                v-for="item in questionTypeList"
                :key="item.value"
                :value="item.value"
                :label="item.label"
              ></el-option>
            </el-select>
          </el-col>
          <el-col :span="6"></el-col>
        </el-row>
      </el-card>
    </div>
  </div>
</template>

<script>
import { simple } from '@/api/hmmm/subjects.js'
import {
  difficulty as difficultyList,
  questionType as questionTypeList
} from '@/api/hmmm/constants' // 按需导入常量数据
export default {
  data() {
    return {
      // 定义各个搜索表单域的数据展示成员
      subjectIDList: [],
      difficultyList,
      questionTypeList,
      // 定义搜索数据对象
      searchForm: {
        subjectID: '',
        difficulty: '',
        questionType: ''
      }
    }
  },
  methods: {
    // 获取学科下拉数据
    async getsubjectIDList() {
      var rst = await simple()
      console.log(rst)
      // 把获得到的学科信息赋予给 subjectIDList 成员
      this.subjectIDList = rst.data
    }
  },
  created() {
    this.getsubjectIDList()
  }
}
</script>

<style scoped>
.wh {
  width: 170px;
}
</style>
