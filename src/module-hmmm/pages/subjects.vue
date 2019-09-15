<template>
  <div class="dashboard-container">
    <div class="app-container">
      <el-card>
        <el-row>
          <el-button class="btn" @click="dialogVisible =true ">新增学科</el-button>
          <i class="el-icon-tickets item1" v-popover:popover style="cursor:help"></i>
          <el-popover
            ref="popover"
            placement="right"
            title="按钮"
            width="200"
            trigger="click"
            v-model="visible"
          >
            <div>
              <b>说明</b>
            </div>
            <div style="margin:5px 0 15px 0">点击显示新建目录弹窗</div>
          </el-popover>
        </el-row>
        <subjects-add :dialogvisible='dialogVisible' @closeDialog='closeDialog' @addSub='addsub'></subjects-add>
        <!-- <el-dialog @close="closedialog" :visible="dialogVisible" width="45%">
          <el-card class="card">
            <div slot="header">
              <strong>新增学科</strong>
            </div>
            <el-form class="form" :model="formData" :rules="formRules" ref="subjectForm">
              <el-form-item label="学科名称" label-width="80px" prop="subjectName">
                <el-input style="width:200px" v-model="formData.subjectName"></el-input>
              </el-form-item>
              <el-form-item label="是否显示">
                <el-switch v-model="formData.isFrontDisplay" active-color="#409EFF"></el-switch>
              </el-form-item>
            </el-form>
          </el-card>
          <span slot="footer" class="dialog-footer">
            <el-button @click="closedialog">取 消</el-button>
            <el-button type="primary" @click="addSubject">确 定</el-button>
          </span>
        </el-dialog> -->
        <el-row type="flex" justify="space-between">
          <span>
            学科名称
            <el-input placeholder="请输入" style="width:200px" v-model="page.subjectName">
              <!-- <i class="el-icon-tickets item"></i> -->
              <!-- <i slot="suffix" class="el-icon-tickets item"></i> -->
            </el-input>
             <i class="el-icon-tickets item" v-popover:popover1 style="cursor:help"></i>
          <el-popover
            ref="popover1"
            placement="right"
            title="检索-学科名称"
            width="200"
            trigger="click"
            v-model="visible1"
          >
          <div><b>说明</b> </div>
          <div style="margin:5px 0 15px 0">1、关键字检索</div>
          </el-popover>
          </span>
          <span>
            <el-button @click="clearSearch">清除</el-button>
            <el-button type="primary" @click="searchSubject">搜索</el-button>
          </span>
        </el-row>
        <el-table :data="list">
          <el-table-column label="序号" type="index" align="center"></el-table-column>
          <el-table-column label="学科名称" prop="subjectName" align="center"></el-table-column>
          <el-table-column label="创建者" prop="username" align="center"></el-table-column>
          <el-table-column label="创建日期" width="180px" align="center">
            <span slot-scope="obj">{{obj.row.addDate | parseTimeByString}}</span>
          </el-table-column>
          <el-table-column label="前台是否显示" align="center">
            <span slot-scope="obja">{{obja.row.isFrontDisplay ?'是':'否'}}</span>
          </el-table-column>
          <el-table-column label="二级目录" prop="twoLevelDirectory" align="center"></el-table-column>
          <el-table-column label="标签" prop="tags" align="center"></el-table-column>
          <el-table-column label="题目数量" prop="totals" align="center"></el-table-column>
          <el-table-column label="创建日期" width="220" align="center">
            <template slot-scope="obj"> 
              <el-button type="text" size="mini">学科分类</el-button>
              <el-button type="text" size="mini">学科标签</el-button>
              <el-button type="text" size="mini">修改</el-button>
              <el-button type="text" size="mini">删除</el-button>
            </template>
          </el-table-column>
        </el-table>
        <el-row type="flex" justify="center">
          <el-pagination
            :current-page="page.page"
            :page-size="page.pagesize"
            :total="page.total"
            @current-change="changePage"
          ></el-pagination>
        </el-row>
      </el-card>
    </div>
  </div>
</template>

<script>
import subjectsAdd from '../components/subjects-add'
import { list as subjectList } from '../../api/hmmm/subjects.js'
export default {
  name: 'SubjectsList',
  components: {
    'subjects-add': subjectsAdd
  },
  data() {
    return {
      visible1: false,
      visible: false,
      // 与验证相关
      // formData: {
      //   subjectName: '',
      //   isFrontDisplay: true
      // },
      // formRules: {
      //   subjectName: [{ required: true, message: '请填写学科名称' }]
      // },
      dialogVisible: false,
      page: {
        page: 1,
        pagesize: 10,
        subjectName: '',
        total: 0
      },
      list: []
    }
  },
  methods: {
    // // 关闭弹框
    // closedialog() {
    //   this.dialogVisible = false
    //   this.formData.subjectName = ''
    //   this.formData.isFrontDisplay = true
    // },
    // // 添加学科
    // addSubject() {
    //   this.$refs.subjectForm.validate(async isOk => {
    //     if (isOk) {
    //       let rst = await add(this.formData)
    //       // console.log(rst)
    //       this.closedialog()
    //       this.getSubjects()
    //     }
    //   })
    // },
    addsub() {
      this.getSubjects()
    },
    closeDialog() {
      this.dialogVisible = false
    },
    clearSearch() {
      this.page.page = 1
      this.page.subjectName = ''
      this.getSubjects()
    },
    searchSubject() {
      this.page.page = 1
      this.getSubjects()
    },
    changePage(newpage) {
      this.page.page = newpage
      this.getSubjects()
    },
    async getSubjects() {
      let rst = await subjectList(this.page)
      this.list = rst.data.items
      this.page.total = rst.data.counts
      console.log(this.list)
      console.log(this.page.total)
    }
  },
  created() {
    this.getSubjects()
  }
}
</script>

<style scoped>
.el-col {
  font-size: 14px;
}
.btn {
  position: relative;
  margin: 10px 0;
}
.item {
  position: absolute;
  top: -8px;
  left: 258;
  color: white;
  background-color: blue;
}
.item1 {
  font-size: 14px;
  position: absolute;
  top: 2px;
  left: 90px;
  color: white;
  background-color: blue;
}
.card {
  margin-top: -20px;
}
.form {
  margin-left: 60px;
  font-weight: normal;
}
</style>
