<template>
  <el-dialog
    :title="!dataForm.id ? '新增' : '修改'"
    :close-on-click-modal="false"
    :visible.sync="visible">
    <el-form :model="dataForm" :rules="dataRule" ref="dataForm" @keyup.enter.native="dataFormSubmit()" label-width="80px">
    <!-- <el-form-item label="用户ID" prop="userId">
      <el-input v-model="dataForm.userId" placeholder="用户ID"></el-input>
    </el-form-item> -->
    <el-form-item label="应用ID" prop="appId">
      <el-input v-model="dataForm.appId" placeholder="应用ID"></el-input>
    </el-form-item>
    <el-form-item label="余额" prop="balance">
      <el-input v-model="dataForm.balance" placeholder="余额"></el-input>
    </el-form-item>
    <el-form-item label="总资产" prop="totalFund">
      <el-input v-model="dataForm.totalFund" placeholder="总资产"></el-input>
    </el-form-item>
    <el-form-item label="冻结资产" prop="frozenFund">
      <el-input v-model="dataForm.frozenFund" placeholder="冻结资产"></el-input>
    </el-form-item>
    <el-form-item label="提现总额" prop="cash">
      <el-input v-model="dataForm.cash" placeholder="提现总额"></el-input>
    </el-form-item>
    <el-form-item label="(1 eos, 2 ETH)" prop="type">
      <el-input v-model="dataForm.type" placeholder="(1 eos, 2 ETH)"></el-input>
    </el-form-item>
    <el-form-item label="钱包状态" prop="status">
      <el-input v-model="dataForm.status" placeholder="钱包状态"></el-input>
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
          balance: '',
          totalFund: '',
          frozenFund: '',
          cash: '',
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
          balance: [
            { required: true, message: '余额不能为空', trigger: 'blur' }
          ],
          totalFund: [
            { required: true, message: '总资产不能为空', trigger: 'blur' }
          ],
          frozenFund: [
            { required: true, message: '冻结资产不能为空', trigger: 'blur' }
          ],
          cash: [
            { required: true, message: '提现总额不能为空', trigger: 'blur' }
          ],
          type: [
            { required: true, message: '(1 eos, 2 ETH)不能为空', trigger: 'blur' }
          ],
          status: [
            { required: true, message: '钱包状态不能为空', trigger: 'blur' }
          ]
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
              url: this.$http.adornUrl(`/app/wallet/info/${this.dataForm.id}`),
              method: 'get',
              params: this.$http.adornParams()
            }).then(({data}) => {
              if (data && data.code === 0) {
                this.dataForm.userId = data.wallet.userId
                this.dataForm.appId = data.wallet.appId
                this.dataForm.balance = data.wallet.balance
                this.dataForm.totalFund = data.wallet.totalFund
                this.dataForm.frozenFund = data.wallet.frozenFund
                this.dataForm.cash = data.wallet.cash
                this.dataForm.type = data.wallet.type
                this.dataForm.status = data.wallet.status
                this.dataForm.createtime = data.wallet.createtime
                this.dataForm.updatetime = data.wallet.updatetime
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
              url: this.$http.adornUrl(`/app/wallet/${!this.dataForm.id ? 'save' : 'update'}`),
              method: 'post',
              data: this.$http.adornData({
                'id': this.dataForm.id || undefined,
                'userId': this.dataForm.userId,
                'appId': this.dataForm.appId,
                'balance': this.dataForm.balance,
                'totalFund': this.dataForm.totalFund,
                'frozenFund': this.dataForm.frozenFund,
                'cash': this.dataForm.cash,
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
