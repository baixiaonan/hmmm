<template>
  <el-dialog @close="closedialog" :visible="dialogvisible" width="45%" class="dial">
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
        </el-dialog>
</template>

<script>
import { add } from '../../api/hmmm/subjects.js'
export default {
  name: 'SubjectsAdd',
  props: ['dialogvisible'],
  data() {
    return {
      closeitem: false,
       formData: {
        subjectName: '',
        isFrontDisplay: true
      },
      formRules: {
        subjectName: [{ required: true, message: '请填写学科名称' }]
      }
    }
  },
  methods: {
     // 关闭弹框
    closedialog() {
      this.$emit('closeDialog')
      // this.dialogvisible = false
      this.formData.subjectName = ''
      this.formData.isFrontDisplay = true
    },
    // 添加学科
    addSubject() {
      this.$refs.subjectForm.validate(async isOk => {
        if (isOk) {
          let rst = await add(this.formData)
          // console.log(rst)
          this.closedialog()
          // this.getSubjects()
          this.$emit('addSub')
        }
      })
    }
  }
}
</script>

<style scoped>
</style>
