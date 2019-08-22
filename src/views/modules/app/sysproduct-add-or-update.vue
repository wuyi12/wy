<template>
  <el-dialog
    :title="!dataForm.id ? '新增' : '修改'"
    :close-on-click-modal="false"
    :visible.sync="visible">
    <el-form :model="dataForm" :rules="dataRule" ref="dataForm" @keyup.enter.native="dataFormSubmit()" label-width="80px">
    <!-- <el-form-item label="用户ID" prop="userId">
      <el-input v-model="dataForm.userId" placeholder="用户ID"></el-input>
    </el-form-item> -->
    <el-form-item label="产品名称" prop="appName">
      <el-input v-model="dataForm.appName" placeholder="产品名称"></el-input>
    </el-form-item>
    <!-- <el-form-item label="产品秘钥" prop="appKey">
      <el-input v-model="dataForm.appKey" placeholder="产品秘钥"></el-input>
    </el-form-item> -->
    <el-form-item label="充值回调" prop="payCb">
      <el-input v-model="dataForm.payCb" placeholder="充值回调"></el-input>
    </el-form-item>
    <el-form-item label="产品描述" prop="appInfo">
      <el-input v-model="dataForm.appInfo" placeholder="产品描述"></el-input>
    </el-form-item>
    </el-form>
    <span slot="footer" class="dialog-footer">
      <el-button @click="visible = false">取消</el-button>
      <el-button type="primary" @click="dataFormSubmit()">确定</el-button>
    </span>
  </el-dialog>
</template>

<script>
  export default {
    data () {
      return {
        visible: false,
        dataForm: {
          appId: 0,
          userId: '',
          appName: '',
          appKey: '',
          payCb: '',
          appInfo: ''
        },
        dataRule: {
          // userId: [
          //   { required: true, message: '用户ID不能为空', trigger: 'blur' }
          // ],
          appName: [
            { required: true, message: '产品名称不能为空', trigger: 'blur' }
          ]
          // appKey: [
          //   { required: true, message: '产品秘钥不能为空', trigger: 'blur' }
          // ],
          // payCb: [
          //   { required: true, message: '充值回调不能为空', trigger: 'blur' }
          // ],
          // appInfo: [
          //   { required: true, message: '产品描述不能为空', trigger: 'blur' }
          // ]
        }
      }
    },
    methods: {
      init (id) {
        this.dataForm.appId = id || 0
        this.visible = true
        this.$nextTick(() => {
          this.$refs['dataForm'].resetFields()
          if (this.dataForm.appId) {
            this.$http({
              url: this.$http.adornUrl(`/app/sysproduct/info/${this.dataForm.appId}`),
              method: 'get',
              params: this.$http.adornParams()
            }).then(({data}) => {
              if (data && data.code === 0) {
                this.dataForm.userId = data.sysproduct.userId
                this.dataForm.appName = data.sysproduct.appName
                this.dataForm.appKey = data.sysproduct.appKey
                this.dataForm.payCb = data.sysproduct.payCb
                this.dataForm.appInfo = data.sysproduct.appInfo
              }
            })
          }
        })
      },
      // 表单提交
      dataFormSubmit () {
        this.$refs['dataForm'].validate((valid) => {
          if (valid) {
            this.$http({
              url: this.$http.adornUrl(`/app/sysproduct/${!this.dataForm.appId ? 'save' : 'update'}`),
              method: 'post',
              data: this.$http.adornData({
                'appId': this.dataForm.appId || undefined,
                'userId': this.dataForm.userId,
                'appName': this.dataForm.appName,
                'appKey': this.dataForm.appKey,
                'payCb': this.dataForm.payCb,
                'appInfo': this.dataForm.appInfo
              })
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
