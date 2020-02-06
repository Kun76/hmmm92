<template>
  <div class="dashboard-container">
    <div class="app-container">
      <el-card class="box-card">
        <!-- :gutter 给各个col设置间歇，单位是像素 -->
        <el-row :gutter="20">
          <el-col :span="6">
            学&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;科:
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
            状&nbsp;&nbsp;&nbsp;&nbsp;态:
            <el-select v-model="searchForm.chkState" class="wh">
              <el-option
                v-for="item in statusList"
                :key="item.value"
                :value="item.value"
                :label="item.label"
              ></el-option>
            </el-select>
          </el-col>
          <el-col :span="6">
            难&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;度:
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
        </el-row>
        <el-row :gutter="20">
          <el-col :span="6">
            标&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;签:
            <el-select v-model="searchForm.tags" class="wh">
              <el-option
                v-for="item in tagsList"
                :key="item.value"
                :value="item.value"
                :label="item.label"
              ></el-option>
            </el-select>
          </el-col>
          <el-col :span="6">
            录入人:
            <el-select v-model="searchForm.creatorID" placeholder="请选择" class="wh">
              <el-option
                v-for="item in creatorIDList"
                :key="item.id"
                :value="item.id"
                :label="item.username"
              ></el-option>
            </el-select>
          </el-col>
          <el-col :span="6">
            关&nbsp;&nbsp;键&nbsp;&nbsp;字:
            <el-input type="text" v-model="searchForm.keyword" class="wh"></el-input>
          </el-col>
          <el-col :span="6">
            题目备注:
            <el-input type="text" v-model="searchForm.remarks" class="wh"></el-input>
          </el-col>
        </el-row>
        <el-row :gutter="20">
          <el-col :span="6">
            二级目录:
            <el-select v-model="searchForm.catalogID" placeholder="请选择" class="wh">
              <el-option
                v-for="item in catalogIDList"
                :key="item.value"
                :label="item.label"
                :value="item.value"
              ></el-option>
            </el-select>
          </el-col>
          <el-col :span="6">
            城&nbsp;&nbsp;&nbsp;&nbsp;市:
            <el-select
              v-model="searchForm.province"
              style="width:90px"
              @change="getcitys(searchForm.province)"
            >
              <el-option v-for="item in provinces()" :key="item" :label="item" :value="item"></el-option>
            </el-select>
            <el-select v-model="searchForm.city" style="width:90px">
              <el-option v-for="item in cityList" :key="item" :label="item" :value="item"></el-option>
            </el-select>
          </el-col>
          <el-col :span="6">
            企业简称:
            <el-input type="text" v-model="searchForm.shortName" class="wh"></el-input>
          </el-col>
          <el-col :span="6">
            方&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;向:
            <el-select v-model="searchForm.direction" placeholder="请选择" class="wh">
              <el-option v-for="item in directionList" :key="item" :value="item" :label="item"></el-option>
            </el-select>
          </el-col>
        </el-row>
        <el-tabs v-model="activeName" type="card">
          <el-tab-pane label="全部" name="first">
            <el-table :data="questionList" style="width:100%"  border>
              <el-table-column label="序号" type="index" width="50"></el-table-column>
              <el-table-column label="试题编号" prop="number" width='120'></el-table-column>
              <el-table-column label="学科" prop="subjectName"></el-table-column>
              <el-table-column label="题型" :formatter="questionTypeFMT" prop="questionType"></el-table-column>
              <el-table-column label="题干" prop="question"></el-table-column>
              <el-table-column label="录入时间" prop="addDate" width="170">
                <span slot-scope="stData">{{stData.row.addDate | parseTimeByString }}</span>
              </el-table-column>
              <el-table-column label="录入人" prop="creator"></el-table-column>
              <el-table-column label="难度" :formatter="difficultyFMT" prop="difficulty"></el-table-column>
              <el-table-column label="使用次数" prop></el-table-column>
              <el-table-column label="审核状态" prop="chkState"></el-table-column>
              <el-table-column label="审核意见" prop='chkRemarks'></el-table-column>
              <el-table-column label="审核人" prop='chkUser'></el-table-column>
              <el-table-column label="发布状态" prop="publishState"></el-table-column>
              <el-table-column label="操作" width="200">
                <a href="#">审核</a>
                <a href="#">预览</a>
                <a href="#">下架</a>
                <a href="#">修改</a>
                <a href="#">删除</a>
              </el-table-column>
            </el-table>
          </el-tab-pane>
          <el-tab-pane label="待审核" name="second">待审核</el-tab-pane>
          <el-tab-pane label="已审核" name="third">已审核</el-tab-pane>
        </el-tabs>
      </el-card>
    </div>
  </div>
</template>

<script>
import { simple } from '@/api/hmmm/subjects.js' // 学科获取方法导入
import { simple as simpletags } from '@/api/hmmm/tags.js' // 标签获取方法导入
import { simple as usersSimple } from '@/api/base/users' // 获取录入人信息方法导入
import { simple as directorysSimple } from '@/api/hmmm/directorys' // 获取二级目录信息方法导入
import { provinces, citys } from '@/api/hmmm/citys' // 获取 省份/城市 信息方法导入
import { choice } from '@/api/hmmm/questions' // 精选题库相关api导入
import {
  direction as directionList,
  difficulty as difficultyList,
  questionType as questionTypeList,
  status as statusList
} from '@/api/hmmm/constants' // 按需导入常量数据 方向,难度,类型,状态
export default {
  name: 'QuestionsChoice',
  data() {
    return {
      activeName: 'first',
      // 定义各个搜索表单域的数据展示成员
      questionList: [], // 精选题库列表信息
      // 定义各个搜索表单域的数据展示成员
      cityList: [],
      chkStateList: [], // 状态
      catalogIDList: [], // 二级目录
      creatorIDList: [], // 录入人
      tagsList: [], // 标签列表
      subjectIDList: [], // 学科
      difficultyList, // 难度
      questionTypeList, // 试题类型
      directionList, // 方向
      statusList, // 状态
      // 定义搜索数据对象
      searchForm: {
        subjectID: '', // 学科
        chkState: '', // 审核状态
        difficulty: '', // 难度
        questionType: '', // 试题类型
        tags: '', // 标签
        province: '', // 城市
        city: '', // 地区
        keyword: '', // 关键字
        remarks: '', // 备注
        shortName: '', // 企业简称
        direction: '', // 方向
        creatorID: '', // 录入人
        catalogID: '' // 二级目录
      }
    }
  },
  methods: {
    // 获取精选题库
    async getQuestionList() {
      // 给list传递数据检索的条件信息
      var result = await choice(this.searchForm)
      this.questionList = result.data.items
      console.log(this.questionList)
    },
    // 难度数字转汉字
    difficultyFMT(row, column, cellValue, index) {
      return this.difficultyList[cellValue - 1].label
    },
    // 题型数字转汉字
    questionTypeFMT(row, column, cellValue, index) {
      return this.questionTypeList[cellValue - 1].label
    },
    provinces, // 获取城市
    // 获取地区
    getcitys(pname) {
      this.searchForm.city = ''
      this.cityList = citys(pname)
    },
    // 获取 二级目录
    async getCatalogIDList() {
      var res = await directorysSimple()
      this.catalogIDList = res.data
    },
    // 获得 录入人 列表数据
    async getCreatorIDList() {
      var res = await usersSimple()
      this.creatorIDList = res.data
    },
    // 获取标签数据
    async getTagsList() {
      var res = await simpletags()
      this.tagsList = res.data
    },
    // 获取学科下拉数据
    async getsubjectIDList() {
      var res = await simple()
      // 把获得到的学科信息赋予给 subjectIDList 成员
      this.subjectIDList = res.data
    }
  },
  created() {
    // 获取学科列表
    this.getsubjectIDList()
    // 获取标签
    this.getTagsList()
    // 获取录入人
    this.getCreatorIDList()
    // 获取二级目录
    this.getCatalogIDList()
    // 获取精选题库
    this.getQuestionList()
  },
  watch: {
    sreachForm: {
      handler: function (newV, oldV) {
         // 获取精选题库
       this.getQuestionList()
      },
      deep: true
    }
  }
}
</script>

<style scoped>
.wh {
  width: 180px;
}
.el-row {
  margin-bottom: 10px;
}
.el-tabs {
  margin-top: 25px;
}
</style>
