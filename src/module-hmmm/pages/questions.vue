<template>
  <div class="dashboard-container">
    <div class="app-container">
      <el-form ref="searchFormRef" :model="searchForm">
        <el-form-item>
          <el-row>
            <el-col :span="6">
              学科:
              <el-select placeholder="请选择" v-model="searchForm.subjectID">
                <el-option v-for="item in subjectIDList" :key="item.value"
                :label="item.label"
                :value="item.value">
                </el-option>
              </el-select>
            </el-col>
            <el-col :span="6">
              难度:
              <el-select placeholder="请选择" v-model="searchForm.difficulty">
                <el-option v-for="item in difficultyList" :key="item.value" 
                :label="item.label"
                :value="item.value"></el-option>
              </el-select>
            </el-col>
            <el-col :span="5">
              试题类型:
              <el-select placeholder="请选择" v-model="searchForm.questionType" style="width:135px">
                <el-option v-for="item in questionTypeList" :key="item.value"
                :label="item.label"
                :value="item.value">

                </el-option>
              </el-select>
            </el-col>
          </el-row>
        </el-form-item>
      </el-form>
    </div>
  </div>
</template>

<script>
import {simple as subjectSimple} from '@/api/hmmm/subjects'
import {difficulty, questionType} from '@/api/hmmm/constants'
export default {
  name: 'QuestionsList',
created() {
  // 获得学科信息
  this.getSubjectIDList()
  // 获得难度信息
  this.difficultyList = difficulty
  this.questionTypeList = questionType
},
  methods: {
    // 获得学科列表数据
    async getSubjectIDList() {
    var result = await subjectSimple()
      console.log(result)
    this.subjectIDList = result.data
    }
  },
  data() {
    return {
      // 基础搜索数据列表部分
      subjectIDList: [],
      difficultyList: [],
      questionTypeList: [],
      // 搜素表单数据对象
      searchForm: {
        subjectID: '', // 学科
        difficulty: '', // 难度
        questionType: '' // 问题类型
      }
    }
  }
}
</script>

<style scoped>
</style>
