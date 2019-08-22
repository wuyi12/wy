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
    <el-form-item label="订单ID" prop="uuid">
      <el-input v-model="dataForm.uuid" placeholder="订单ID"></el-input>
    </el-form-item>
    <el-form-item label="数量" prop="amount">
      <el-input v-model="dataForm.amount" placeholder="数量"></el-input>
    </el-form-item>
    <el-form-item label="备注" prop="memo">
      <el-input v-model="dataForm.memo" placeholder="备注"></el-input>
    </el-form-item>
    <el-form-item label="提币地址" prop="addr">
      <el-input v-model="dataForm.addr" placeholder="提币地址"></el-input>
    </el-form-item>
    <el-form-item label="币种" prop="type">
      <el-input v-model="dataForm.type" placeholder="币种"></el-input>
    </el-form-item>
    <el-form-item label="手续费" prop="tip">
      <el-input v-model="dataForm.tip" placeholder="手续费"></el-input>
    </el-form-item>
    <el-form-item label="状态" prop="status">
      <el-input v-model="dataForm.status" placeholder="状态"></el-input>
    </el-form-item>
    <el-form-item label="人工审查" prop="isCheck">
      <el-input v-model="dataForm.isCheck" placeholder="人工审查"></el-input>
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
          uuid: '',
          amount: '',
          memo: '',
          addr: '',
          type: '',
          tip: '',
          status: '',
          isCheck: '',
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
          uuid: [
            { required: true, message: '订单ID不能为空', trigger: 'blur' }
          ],
          amount: [
            { required: true, message: '数量不能为空', trigger: 'blur' }
          ],
          // memo: [
          //   { required: true, message: '备注不能为空', trigger: 'blur' }
          // ],
          addr: [
            { required: true, message: '提币地址不能为空', trigger: 'blur' }
          ],
          type: [
            { required: true, message: '币种不能为空', trigger: 'blur' }
          ],
          tip: [
            { required: true, message: '手续费不能为空', trigger: 'blur' }
          ],
          status: [
            { required: true, message: '状态不能为空', trigger: 'blur' }
          ]
          // isCheck: [
          //   { required: true, message: '人工审查不能为空', trigger: 'blur' }
          // ],
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
              url: this.$http.adornUrl(`/app/withdraw/info/${this.dataForm.id}`),
              method: 'get',
              params: this.$http.adornParams()
            }).then(({data}) => {
              if (data && data.code === 0) {
                this.dataForm.userId = data.withdraw.userId
                this.dataForm.appId = data.withdraw.appId
                this.dataForm.uuid = data.withdraw.uuid
                this.dataForm.amount = data.withdraw.amount
                this.dataForm.memo = data.withdraw.memo
                this.dataForm.addr = data.withdraw.addr
                this.dataForm.type = data.withdraw.type
                this.dataForm.tip = data.withdraw.tip
                this.dataForm.status = data.withdraw.status
                this.dataForm.isCheck = data.withdraw.isCheck
                this.dataForm.createtime = data.withdraw.createtime
                this.dataForm.updatetime = data.withdraw.updatetime
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
              url: this.$http.adornUrl(`/app/withdraw/${!this.dataForm.id ? 'save' : 'update'}`),
              method: 'post',
              data: this.$http.adornData({
                'id': this.dataForm.id || undefined,
                'userId': this.dataForm.userId,
                'appId': this.dataForm.appId,
                'uuid': this.dataForm.uuid,
                'amount': this.dataForm.amount,
                'memo': this.dataForm.memo,
                'addr': this.dataForm.addr,
                'type': this.dataForm.type,
                'tip': this.dataForm.tip,
                'status': this.dataForm.status,
                'isCheck': this.dataForm.isCheck,
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
