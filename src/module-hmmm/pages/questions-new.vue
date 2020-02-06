<template>
  <div class="dashboard-container">
    <div class="app-container">
      <el-card class="box-card">
        <el-form ref="addFormRef" :model="addForm" label-width="120px" :rules="addFormRules">
          <!--:model作用：可以使得el-form针对表单的全部信息进行收集，固定属性-->
          <el-form-item label="学科：" prop="subjectID">
            <el-select v-model="addForm.subjectID">
              <el-option
                v-for="item in subjectIDList"
                :key="item.value"
                :value="item.value"
                :label="item.label"
              ></el-option>
            </el-select>
          </el-form-item>
          <el-form-item label="目录：" prop="catalogID">
            <el-select v-model="addForm.catalogID">
              <el-option
                v-for="item in catalogIDList"
                :key="item.value"
                :label="item.label"
                :value="item.value"
              ></el-option>
            </el-select>
          </el-form-item>
          <el-form-item label="企业：" prop="enterpriseID">
            <el-select v-model="addForm.enterpriseID">
              <el-option
                v-for="item in enterpriseIDList"
                :key="item.id"
                :label="item.shortName"
                :value="item.id"
              ></el-option>
            </el-select>
          </el-form-item>
          <el-form-item label="城市：" prop="province">
            <el-select v-model="addForm.province" @change="addForm.city=''">
              <el-option v-for="item in provinces()" :key="item" :label="item" :value="item"></el-option>
            </el-select>
          </el-form-item>
          <el-form-item prop="city">
            <el-select v-model="addForm.city">
              <el-option
                v-for="item in citys(addForm.province)"
                :key="item"
                :label="item"
                :value="item"
              ></el-option>
            </el-select>
          </el-form-item>
          <el-form-item label="方向：" prop="direction">
            <el-select v-model="addForm.direction">
              <el-option v-for="item in directionList" :key="item" :label="item" :value="item"></el-option>
            </el-select>
          </el-form-item>
          <el-form-item label="题型：" prop="questionType">
            <el-radio-group v-model="addForm.questionType">
              <el-radio
                v-for="item in questionTypeList"
                :key="item.value"
                :label=" item.value+'' "
              >{{item.label}}</el-radio>
            </el-radio-group>
          </el-form-item>
          <el-form-item label="难度：" prop="difficulty">
            <el-radio-group v-model="addForm.difficulty">
              <el-radio
                v-for="item in difficultyList"
                :key="item.value"
                :label=" item.value+'' "
              >{{item.label}}</el-radio>
            </el-radio-group>
          </el-form-item>
          <el-form-item label="题干：" prop="question">
            <el-input type="textarea" v-model="addForm.question"></el-input>
          </el-form-item>
          <el-form-item label="选项" prop="options">
            <el-radio :label="0" v-model="singleSelect">
              A:
              <el-input type="text" v-model="addForm.options[0]['title']"></el-input>
            </el-radio>
            <br />
            <el-radio :label="1" v-model="singleSelect">
              B:
              <el-input type="text" v-model="addForm.options[1]['title']"></el-input>
            </el-radio>
            <br />
            <el-radio :label="2" v-model="singleSelect">
              C:
              <el-input type="text" v-model="addForm.options[2]['title']"></el-input>
            </el-radio>
            <br />
            <el-radio :label="3" v-model="singleSelect">
              D:
              <el-input type="text" v-model="addForm.options[3]['title']"></el-input>
            </el-radio>
            <br />
          </el-form-item>
          <el-form-item label="答案：" prop="answer">
            <el-input type="textarea" v-model="addForm.answer"></el-input>
          </el-form-item>
          <el-form-item label="备注：" prop="remarks">
            <el-input type="textarea" v-model="addForm.remarks"></el-input>
          </el-form-item>
          <el-form-item label="标签：" prop="tags">
            <el-input type="text" v-model="addForm.tags"></el-input>
          </el-form-item>
          <el-form-item>
            <el-button type="primary" @click="addqs()">提交</el-button>
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
import { list } from '@/api/hmmm/companys' // 企业数据
import { add } from '@/api/hmmm/questions' // 试题添加
import {
  difficulty as difficultyList,
  direction as directionList,
  questionType as questionTypeList
} from '@/api/hmmm/constants' // 常量(难度,方向,题型)
export default {
  name: 'QuestionsNew',
  data() {
    return {
      difficultyList, // 难度
      enterpriseIDList: [], // 企业
      subjectIDList: [], // 学科
      catalogIDList: [], // 二级目录
      directionList, // 方向(简易成员赋值)
      questionTypeList, // 题型
      // 感知被被选中的项目的值，是中间成员，需要通过watch转变为isRight
      singleSelect: '',
      // 添加试题 表单数据对象
      addForm: {
        subjectID: '', // 学科
        catalogID: '', // 二级目录
        enterpriseID: '', // 企业
        city: '', // 区县
        province: '', // 城市
        direction: '', // 方向
        questionType: '1', // 默认“单选” 题型 项目被选中(要求是字符串)
        difficulty: '1', // 难度
        question: '', // 题干
        answer: '', // 答案
        remarks: '', // 备注
        tags: '', // 标签
        videoURL: 'http://www.xxx.com', // 解析视频
        options: [
          { code: 'A', title: '', img: '', isRight: false },
          { code: 'B', title: '', img: '', isRight: false },
          { code: 'C', title: '', img: '', isRight: false },
          { code: 'D', title: '', img: '', isRight: false }
        ]
      },
      // 表单验证
      addFormRules: {
        subjectID: [{ required: true, message: '请选择学科', trigger: 'blur' }], // 学科
        catalogID: [{ required: true, message: '请选择目录', trigger: 'blur' }], // 二级目录
        enterpriseID: [
          { required: true, message: '请选择企业', trigger: 'blur' }
        ], // 企业
        city: [{ required: true, message: '请选择区县', trigger: 'blur' }], // 区县
        province: [{ required: true, message: '请选择城市', trigger: 'blur' }], // 城市
        direction: [{ required: true, message: '请选择方向', trigger: 'blur' }], // 方向
        question: [{ required: true, message: '请填写题干', trigger: 'blur' }], // 题干
        answer: [{ required: true, message: '请填写答案', trigger: 'blur' }], // 答案
        remarks: [{ required: true, message: '请填写备注', trigger: 'blur' }], // 备注
        tags: [{ required: true, message: '请填写标签', trigger: 'blur' }], // 标签
        options: [{ required: true, message: '请填写标签', trigger: 'blur' }] 
      }
    }
  },
  methods: {
    provinces, // 简体成员赋值 城市
    citys, // 县区
    // 添加功能
    addqs() {
      this.$refs.addFormRef.validate(async valid => {
        if (!valid) {
          this.$message.error('请正常填写')
          return false
        }
        await add(this.addForm)
        this.$message.success('添加基础试题成功!')
        this.$router.push('/questions/list')
      })
    },

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
  },
  watch: {
    singleSelect: function(newV, oldV) {
      // 先把全部设为false
      for (let i = 0; i < this.addForm.options.length; i++) {
        this.addForm.options[i].isRight = false
      }
      this.addForm.options[newV].isRight = true
    }
  }
}
</script>

<style scoped>
</style>
