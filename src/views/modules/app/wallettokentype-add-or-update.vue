<template>
  <el-dialog
    :title="!dataForm.id ? '新增' : '修改'"
    :close-on-click-modal="false"
    :visible.sync="visible">
    <el-form :model="dataForm" :rules="dataRule" ref="dataForm" @keyup.enter.native="dataFormSubmit()" label-width="80px">
    <el-form-item label="币符号" prop="tokenSymbol">
      <el-input v-model="dataForm.tokenSymbol" placeholder="币符号"></el-input>
    </el-form-item>
    <el-form-item label="合约地址" prop="tokenAddr">
      <el-input v-model="dataForm.tokenAddr" placeholder="合约地址"></el-input>
    </el-form-item>
    <el-form-item label="币名字" prop="tokenName">
      <el-input v-model="dataForm.tokenName" placeholder="币名字"></el-input>
    </el-form-item>
    <el-form-item label="币或者合约状态（1 正常）" prop="tokenStatus">
      <el-input v-model="dataForm.tokenStatus" placeholder="币或者合约状态（1 正常）"></el-input>
    </el-form-item>
    <el-form-item label="创建时间" prop="createtime">
      <el-input v-model="dataForm.createtime" placeholder="创建时间"></el-input>
    </el-form-item>
    <el-form-item label="更新时间" prop="updatetime">
      <el-input v-model="dataForm.updatetime" placeholder="更新时间"></el-input>
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
          tokenId: 0,
          tokenSymbol: '',
          tokenAddr: '',
          tokenName: '',
          tokenStatus: '',
          createtime: '',
          updatetime: ''
        },
        dataRule: {
          tokenSymbol: [
            { required: true, message: '币符号不能为空', trigger: 'blur' }
          ],
          tokenAddr: [
            { required: true, message: '合约地址不能为空', trigger: 'blur' }
          ],
          tokenName: [
            { required: true, message: '币名字不能为空', trigger: 'blur' }
          ],
          tokenStatus: [
            { required: true, message: '币或者合约状态（1 正常）不能为空', trigger: 'blur' }
          ],
          createtime: [
            { required: true, message: '创建时间不能为空', trigger: 'blur' }
          ],
          updatetime: [
            { required: true, message: '更新时间不能为空', trigger: 'blur' }
          ]
        }
      }
    },
    methods: {
      init (id) {
        this.dataForm.tokenId = id || 0
        this.visible = true
        this.$nextTick(() => {
          this.$refs['dataForm'].resetFields()
          if (this.dataForm.tokenId) {
            this.$http({
              url: this.$http.adornUrl(`/app/wallettokentype/info/${this.dataForm.tokenId}`),
              method: 'get',
              params: this.$http.adornParams()
            }).then(({data}) => {
              if (data && data.code === 0) {
                this.dataForm.tokenSymbol = data.wallettokentype.tokenSymbol
                this.dataForm.tokenAddr = data.wallettokentype.tokenAddr
                this.dataForm.tokenName = data.wallettokentype.tokenName
                this.dataForm.tokenStatus = data.wallettokentype.tokenStatus
                this.dataForm.createtime = data.wallettokentype.createtime
                this.dataForm.updatetime = data.wallettokentype.updatetime
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
              url: this.$http.adornUrl(`/app/wallettokentype/${!this.dataForm.tokenId ? 'save' : 'update'}`),
              method: 'post',
              data: this.$http.adornData({
                'tokenId': this.dataForm.tokenId || undefined,
                'tokenSymbol': this.dataForm.tokenSymbol,
                'tokenAddr': this.dataForm.tokenAddr,
                'tokenName': this.dataForm.tokenName,
                'tokenStatus': this.dataForm.tokenStatus,
                'createtime': this.dataForm.createtime,
                'updatetime': this.dataForm.updatetime
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
