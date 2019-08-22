<template>
  <el-dialog
    :title="!dataForm.id ? '新增' : '修改'"
    :close-on-click-modal="false"
    :visible.sync="visible">
    <el-form :model="dataForm" :rules="dataRule" ref="dataForm" @keyup.enter.native="dataFormSubmit()" label-width="80px">
    <el-form-item label="应用ID" prop="appId">
      <el-input v-model="dataForm.appId" placeholder="应用ID"></el-input>
    </el-form-item>
    <el-form-item label="用户ID(获得奖励者)" prop="userId">
      <el-input v-model="dataForm.userId" placeholder="用户ID(获得奖励者)"></el-input>
    </el-form-item>
    <el-form-item label="游戏种类" prop="game">
      <el-input v-model="dataForm.game" placeholder="游戏种类"></el-input>
    </el-form-item>
    <el-form-item label="玩家ID" prop="playerId">
      <el-input v-model="dataForm.playerId" placeholder="玩家ID"></el-input>
    </el-form-item>
    <el-form-item label="玩家姓名" prop="playerName">
      <el-input v-model="dataForm.playerName" placeholder="玩家姓名"></el-input>
    </el-form-item>
    <el-form-item label="邀请等级" prop="referLeave">
      <el-input v-model="dataForm.referLeave" placeholder="邀请等级"></el-input>
    </el-form-item>
    <el-form-item label="下注数量" prop="betAmount">
      <el-input v-model="dataForm.betAmount" placeholder="下注数量"></el-input>
    </el-form-item>
    <el-form-item label="奖励" prop="reward">
      <el-input v-model="dataForm.reward" placeholder="奖励"></el-input>
    </el-form-item>
    <el-form-item label="币种" prop="type">
      <el-input v-model="dataForm.type" placeholder="币种"></el-input>
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
          appId: '',
          userId: '',
          game: '',
          playerId: '',
          playerName: '',
          referLeave: '',
          betAmount: '',
          reward: '',
          type: '',
          status: '',
          createtime: '',
          updatetime: ''
        },
        dataRule: {
          appId: [
            { required: true, message: '应用ID不能为空', trigger: 'blur' }
          ],
          userId: [
            { required: true, message: '用户ID(获得奖励者)不能为空', trigger: 'blur' }
          ],
          game: [
            { required: true, message: '游戏种类不能为空', trigger: 'blur' }
          ],
          playerId: [
            { required: true, message: '玩家ID不能为空', trigger: 'blur' }
          ],
          playerName: [
            { required: true, message: '玩家姓名不能为空', trigger: 'blur' }
          ],
          referLeave: [
            { required: true, message: '邀请等级不能为空', trigger: 'blur' }
          ],
          betAmount: [
            { required: true, message: '下注数量不能为空', trigger: 'blur' }
          ],
          reward: [
            { required: true, message: '奖励不能为空', trigger: 'blur' }
          ],
          type: [
            { required: true, message: '币种不能为空', trigger: 'blur' }
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
              url: this.$http.adornUrl(`/generator/referdetail/info/${this.dataForm.id}`),
              method: 'get',
              params: this.$http.adornParams()
            }).then(({data}) => {
              if (data && data.code === 0) {
                this.dataForm.appId = data.referdetail.appId
                this.dataForm.userId = data.referdetail.userId
                this.dataForm.game = data.referdetail.game
                this.dataForm.playerId = data.referdetail.playerId
                this.dataForm.playerName = data.referdetail.playerName
                this.dataForm.referLeave = data.referdetail.referLeave
                this.dataForm.betAmount = data.referdetail.betAmount
                this.dataForm.reward = data.referdetail.reward
                this.dataForm.type = data.referdetail.type
                this.dataForm.status = data.referdetail.status
                this.dataForm.createtime = data.referdetail.createtime
                this.dataForm.updatetime = data.referdetail.updatetime
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
              url: this.$http.adornUrl(`/generator/referdetail/${!this.dataForm.id ? 'save' : 'update'}`),
              method: 'post',
              data: this.$http.adornData({
                'id': this.dataForm.id || undefined,
                'appId': this.dataForm.appId,
                'userId': this.dataForm.userId,
                'game': this.dataForm.game,
                'playerId': this.dataForm.playerId,
                'playerName': this.dataForm.playerName,
                'referLeave': this.dataForm.referLeave,
                'betAmount': this.dataForm.betAmount,
                'reward': this.dataForm.reward,
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
