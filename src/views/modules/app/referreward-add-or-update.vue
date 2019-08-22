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
    <el-form-item label="用户名字" prop="userName">
      <el-input v-model="dataForm.userName" placeholder="用户名字"></el-input>
    </el-form-item>
    <el-form-item label="一级邀请奖励" prop="referReward1">
      <el-input v-model="dataForm.referReward1" placeholder="一级邀请奖励"></el-input>
    </el-form-item>
    <el-form-item label="二级邀请奖励" prop="referReward2">
      <el-input v-model="dataForm.referReward2" placeholder="二级邀请奖励"></el-input>
    </el-form-item>
    <el-form-item label="三级邀请奖励" prop="referReward3">
      <el-input v-model="dataForm.referReward3" placeholder="三级邀请奖励"></el-input>
    </el-form-item>
    <el-form-item label="(1 eos, 2 ETH, 3 USDT, 4 TRX)" prop="type">
      <el-input v-model="dataForm.type" placeholder="(1 eos, 2 ETH, 3 USDT, 4 TRX)"></el-input>
    </el-form-item>
    <el-form-item label="状态" prop="status">
      <el-input v-model="dataForm.status" placeholder="状态"></el-input>
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
          id: 0,
          userId: '',
          appId: '',
          userName: '',
          referReward1: '',
          referReward2: '',
          referReward3: '',
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
          userName: [
            { required: true, message: '用户名字不能为空', trigger: 'blur' }
          ],
          referReward1: [
            { required: true, message: '一级邀请奖励不能为空', trigger: 'blur' }
          ],
          referReward2: [
            { required: true, message: '二级邀请奖励不能为空', trigger: 'blur' }
          ],
          referReward3: [
            { required: true, message: '三级邀请奖励不能为空', trigger: 'blur' }
          ],
          type: [
            { required: true, message: '(1 eos, 2 ETH, 3 USDT, 4 TRX)不能为空', trigger: 'blur' }
          ],
          status: [
            { required: true, message: '状态不能为空', trigger: 'blur' }
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
        this.dataForm.id = id || 0
        this.visible = true
        this.$nextTick(() => {
          this.$refs['dataForm'].resetFields()
          if (this.dataForm.id) {
            this.$http({
              url: this.$http.adornUrl(`/generator/referreward/info/${this.dataForm.id}`),
              method: 'get',
              params: this.$http.adornParams()
            }).then(({data}) => {
              if (data && data.code === 0) {
                this.dataForm.userId = data.referreward.userId
                this.dataForm.appId = data.referreward.appId
                this.dataForm.userName = data.referreward.userName
                this.dataForm.referReward1 = data.referreward.referReward1
                this.dataForm.referReward2 = data.referreward.referReward2
                this.dataForm.referReward3 = data.referreward.referReward3
                this.dataForm.type = data.referreward.type
                this.dataForm.status = data.referreward.status
                this.dataForm.createtime = data.referreward.createtime
                this.dataForm.updatetime = data.referreward.updatetime
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
              url: this.$http.adornUrl(`/generator/referreward/${!this.dataForm.id ? 'save' : 'update'}`),
              method: 'post',
              data: this.$http.adornData({
                'id': this.dataForm.id || undefined,
                'userId': this.dataForm.userId,
                'appId': this.dataForm.appId,
                'userName': this.dataForm.userName,
                'referReward1': this.dataForm.referReward1,
                'referReward2': this.dataForm.referReward2,
                'referReward3': this.dataForm.referReward3,
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
