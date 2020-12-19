<template>
  <el-dialog
    :title="!dataForm.id ? '新增' : '修改'"
    :close-on-click-modal="false"
    :visible.sync="visible">
    <el-form :model="dataList" :rules="dataRule" ref="dataForm"  label-width="80px">
      <el-form-item label="文章标题" prop="articleTitle" :class="{ 'is-required': !dataForm.id }">
        <el-input v-model="dataList.articleTitle" placeholder="文章题目" maxlength="30" type="text" show-word-limit></el-input>
      </el-form-item>
      <el-form-item label="文章内容" prop="articleContent" :class="{ 'is-required': !dataForm.id }">
        <el-input v-model="dataList.articleContent" type="textarea" placeholder="文章内容" rows=20></el-input>
      </el-form-item>
      
     
      
    </el-form>
    <span slot="footer" class="dialog-footer">
      <el-button @click="visible = false">取消</el-button>
      <el-button type="primary" @click="dataFormSubmit()">确定</el-button>
    </span>
  </el-dialog>
</template>

<script>
  // import { isEmail, isMobile } from '@/utils/validate'
  export default {
    data () {
      return {
        visible: false,
        dataList: {},
        dataForm: {
          id: 0
        },
        dataRule: {
          articleTitle: [
            { required: true, message: '文章标题不能为空', trigger: 'blur' }
          ],
          articleContent: [
            { required: true, message: '文章内容不能为空', trigger: 'blur' }
          ]
        }
      }
    },
    methods: {
      init (articleId) {
        console.log('是否新增' + (!this.dataForm.id))
        console.log('articleId=' + articleId)
        this.dataForm.id = articleId || 0
        console.log('dataForm.id = ' + this.dataForm.id)
        // alert('是否新增' + this.dataForm.id)
        this.$http({
          url: this.$http.adornUrl(`/article/getArticleById/${articleId}`),
          method: 'get',
          params: this.$http.adornParams()
        }).then(({data}) => {
          this.dataList = data && data.code === 0 ? data.ArticleEntity : {}
        }).then(() => {
          this.visible = true
          // this.$nextTick(() => {
          //   this.$refs['dataForm'].resetFields()
          // })
        })
      },
      // 表单提交
      dataFormSubmit () {
        alert('id = ' + this.dataForm.id)
        this.$refs['dataForm'].validate((valid) => {
          if (valid) {
            this.$http({
              // url: this.$http.adornUrl(`/sys/user/${!this.dataForm.id ? 'save' : 'update'}`),
              url: this.$http.adornUrl(`/article/${!this.dataForm.id ? 'add' : 'updateArticle'}`),
              method: 'post',
              data: this.$http.adornData({
                'userId': 1,
                'articleId': this.dataList.articleId,
                'articleTitle': this.dataList.articleTitle,
                'articleContent': this.dataList.articleContent
              }, false)
            }).then(({data}) => {
              if (data && data.code === 0) {
                this.$message({
                  message: '操作成功',
                  type: 'success',
                  duration: 1500,
                  onClose: () => {
                    this.visible = false
                    this.$emit('refreshDataList')
                  }
                })
              } else {
                this.$message.error(data.msg)
              }
            })
          }
        })
      }
    }
  }
</script>
