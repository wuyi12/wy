<template>
  <el-dialog
    :title="!dataForm.id ? '新增' : '修改'"
    :close-on-click-modal="false"
    :visible.sync="visible">
    <el-form :model="dataForm" :rules="dataRule" ref="dataForm" @keyup.enter.native="dataFormSubmit()" label-width="80px">
    <el-form-item label="用户ID" prop="userId">
      <el-input v-model="dataForm.userId" placeholder="用户ID"></el-input>
    </el-form-item>
    <el-form-item label="应用ID" prop="appId">
      <el-input v-model="dataForm.appId" placeholder="应用ID"></el-input>
    </el-form-item>
    <el-form-item label="谁充值的" prop="from">
      <el-input v-model="dataForm.from" placeholder="谁充值的"></el-input>
    </el-form-item>
    <el-form-item label="交易hash" prop="hash">
      <el-input v-model="dataForm.hash" placeholder="交易hash"></el-input>
    </el-form-item>
    <el-form-item label="交易区块号" prop="blockNum">
      <el-input v-model="dataForm.blockNum" placeholder="交易区块号"></el-input>
    </el-form-item>
    <el-form-item label="数量" prop="amount">
      <el-input v-model="dataForm.amount" placeholder="数量"></el-input>
    </el-form-item>
    <el-form-item label="(1 eos, 2 ETH, 3 USDT 4, TRX)" prop="type">
      <el-input v-model="dataForm.type" placeholder="(1 eos, 2 ETH, 3 USDT 4, TRX)"></el-input>
    </el-form-item>
    <el-form-item label="状态" prop="status">
      <el-input v-model="dataForm.status" placeholder="状态"></el-input>
    </el-form-item>
    <!-- <el-form-item label="创建时间" prop="createtime">
      <el-input v-model="dataForm.createtime" placeholder="创建时间"></el-input>
    </el-form-item>
    <el-form-item label="更新时间" prop="updatetime">
      <el-input v-model="dataForm.updatetime" placeholder="更新时间"></el-input>
    </el-form-item> -->
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
          id: 0,
          userId: '',
          appId: '',
          from: '',
          hash: '',
          blockNum: '',
          amount: '',
          type: '',
          status: '',
          createtime: '',
          updatetime: ''
        },
        dataRule: {
          userId: [
            { required: true, message: '用户ID不能为空', trigger: 'blur' }
          ],
          appId: [
            { required: true, message: '应用ID不能为空', trigger: 'blur' }
          ],
          from: [
            { required: true, message: '谁充值的不能为空', trigger: 'blur' }
          ],
          hash: [
            { required: true, message: '交易hash不能为空', trigger: 'blur' }
          ],
          // blockNum: [
          //   { required: true, message: '交易区块号不能为空', trigger: 'blur' }
          // ],
          amount: [
            { required: true, message: '数量不能为空', trigger: 'blur' }
          ],
          type: [
            { required: true, message: '(1 eos, 2 ETH, 3 USDT 4, TRX)不能为空', trigger: 'blur' }
          ],
          status: [
            { required: true, message: '状态不能为空', trigger: 'blur' }
          ],
          // createtime: [
          //   { required: true, message: '创建时间不能为空', trigger: 'blur' }
          // ],
          // updatetime: [
          //   { required: true, message: '更新时间不能为空', trigger: 'blur' }
          // ]
        }
      }
    },
    methods: {
      init (id) {
        this.dataForm.id = id || 0
        this.visible = true
        this.$nextTick(() => {
          this.$refs['dataForm'].resetFields()
          if (this.dataForm.id) {
            this.$http({
              url: this.$http.adornUrl(`/app/recharge/info/${this.dataForm.id}`),
              method: 'get',
              params: this.$http.adornParams()
            }).then(({data}) => {
              if (data && data.code === 0) {
                this.dataForm.userId = data.recharge.userId
                this.dataForm.appId = data.recharge.appId
                this.dataForm.from = data.recharge.from
                this.dataForm.hash = data.recharge.hash
                this.dataForm.blockNum = data.recharge.blockNum
                this.dataForm.amount = data.recharge.amount
                this.dataForm.type = data.recharge.type
                this.dataForm.status = data.recharge.status
                this.dataForm.createtime = data.recharge.createtime
                this.dataForm.updatetime = data.recharge.updatetime
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
              url: this.$http.adornUrl(`/app/recharge/${!this.dataForm.id ? 'save' : 'update'}`),
              method: 'post',
              data: this.$http.adornData({
                'id': this.dataForm.id || undefined,
                'userId': this.dataForm.userId,
                'appId': this.dataForm.appId,
                'from': this.dataForm.from,
                'hash': this.dataForm.hash,
                'blockNum': this.dataForm.blockNum,
                'amount': this.dataForm.amount,
                'type': this.dataForm.type,
                'status': this.dataForm.status,
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
