<template>
  <el-dialog
    :title="!dataForm.id ? '新增' : '修改'"
    :close-on-click-modal="false"
    :visible.sync="visible">
    <el-form :model="dataForm" :rules="dataRule" ref="dataForm" @keyup.enter.native="dataFormSubmit()" label-width="80px">
    <el-form-item label="电话" prop="phone">
      <el-input v-model="dataForm.phone" placeholder="电话"></el-input>
    </el-form-item>
    <el-form-item label="应用ID" prop="appId">
      <el-input v-model="dataForm.appId" placeholder="应用ID"></el-input>
    </el-form-item>
    <el-form-item label="昵称" prop="nick">
      <el-input v-model="dataForm.nick" placeholder="昵称"></el-input>
    </el-form-item>
    <!-- <el-form-item label="密码" prop="password">
      <el-input v-model="dataForm.password" placeholder="密码"></el-input>
    </el-form-item>
    <el-form-item label="钱包密码" prop="walletPassword">
      <el-input v-model="dataForm.walletPassword" placeholder="钱包密码"></el-input>
    </el-form-item>
    <el-form-item label="邮箱" prop="email">
      <el-input v-model="dataForm.email" placeholder="邮箱"></el-input>
    </el-form-item>
    <el-form-item label="头像" prop="portrait">
      <el-input v-model="dataForm.portrait" placeholder="头像"></el-input>
    </el-form-item> -->
    <el-form-item label="邀请码" prop="referCode">
      <el-input v-model="dataForm.referCode" placeholder="邀请码"></el-input>
    </el-form-item>
    <el-form-item label="1：正常用户，0：禁用用户" prop="status">
      <el-input v-model="dataForm.status" placeholder="1：正常用户，0：禁用用户"></el-input>
    </el-form-item>
    <!-- <el-form-item label="创建时间" prop="createtime">
      <el-input v-model="dataForm.createtime" placeholder="创建时间"></el-input>
    </el-form-item>
    <el-form-item label="更新时间" prop="updatetime">
      <el-input v-model="dataForm.updatetime" placeholder="更新时间"></el-input>
    </el-form-item> -->
    <!-- <el-form-item label="真实姓名" prop="realName">
      <el-input v-model="dataForm.realName" placeholder="真实姓名"></el-input>
    </el-form-item>
    <el-form-item label="身份证号" prop="idCard">
      <el-input v-model="dataForm.idCard" placeholder="身份证号"></el-input>
    </el-form-item> -->
    <!-- <el-form-item label="二级密码" prop="secondPassword">
      <el-input v-model="dataForm.secondPassword" placeholder="二级密码"></el-input>
    </el-form-item> -->
    <!-- <el-form-item label="支付宝账号" prop="alipay">
      <el-input v-model="dataForm.alipay" placeholder="支付宝账号"></el-input>
    </el-form-item>
    <el-form-item label="支付宝收款码图片存储路径" prop="alipayPic">
      <el-input v-model="dataForm.alipayPic" placeholder="支付宝收款码图片存储路径"></el-input>
    </el-form-item> -->
    <el-form-item label="手机号前缀" prop="countryCode">
      <el-input v-model="dataForm.countryCode" placeholder="手机号前缀"></el-input>
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
          userId: 0,
          phone: '',
          appId: '',
          nick: '',
          password: '',
          walletPassword: '',
          email: '',
          portrait: '',
          referCode: '',
          status: '',
          createtime: '',
          updatetime: '',
          realName: '',
          idCard: '',
          secondPassword: '',
          alipay: '',
          alipayPic: '',
          countryCode: ''
        },
        dataRule: {
          phone: [
            { required: true, message: '电话不能为空', trigger: 'blur' }
          ],
          appId: [
            { required: true, message: '应用ID不能为空', trigger: 'blur' }
          ],
          // nick: [
          //   { required: true, message: '昵称不能为空', trigger: 'blur' }
          // ],
          // password: [
          //   { required: true, message: '密码不能为空', trigger: 'blur' }
          // ],
          // walletPassword: [
          //   { required: true, message: '钱包密码不能为空', trigger: 'blur' }
          // ],
          // email: [
          //   { required: true, message: '邮箱不能为空', trigger: 'blur' }
          // ],
          // portrait: [
          //   { required: true, message: '头像不能为空', trigger: 'blur' }
          // ],
          referCode: [
            { required: true, message: '邀请码不能为空', trigger: 'blur' }
          ],
          status: [
            { required: true, message: '1：正常用户，0：禁用用户不能为空', trigger: 'blur' }
          ],
          // createtime: [
          //   { required: true, message: '创建时间不能为空', trigger: 'blur' }
          // ],
          // updatetime: [
          //   { required: true, message: '更新时间不能为空', trigger: 'blur' }
          // ],
          // realName: [
          //   { required: true, message: '真实姓名不能为空', trigger: 'blur' }
          // ],
          // idCard: [
          //   { required: true, message: '身份证号不能为空', trigger: 'blur' }
          // ],
          // secondPassword: [
          //   { required: true, message: '二级密码不能为空', trigger: 'blur' }
          // ],
          // alipay: [
          //   { required: true, message: '支付宝账号不能为空', trigger: 'blur' }
          // ],
          // alipayPic: [
          //   { required: true, message: '支付宝收款码图片存储路径不能为空', trigger: 'blur' }
          // ],
          countryCode: [
            { required: true, message: '手机号前缀不能为空', trigger: 'blur' }
          ]
        }
      }
    },
    methods: {
      init (id) {
        this.dataForm.userId = id || 0
        this.visible = true
        this.$nextTick(() => {
          this.$refs['dataForm'].resetFields()
          if (this.dataForm.userId) {
            this.$http({
              url: this.$http.adornUrl(`/app/user/info/${this.dataForm.userId}`),
              method: 'get',
              params: this.$http.adornParams()
            }).then(({data}) => {
              if (data && data.code === 0) {
                this.dataForm.phone = data.user.phone
                this.dataForm.appId = data.user.appId
                this.dataForm.nick = data.user.nick
                this.dataForm.password = data.user.password
                this.dataForm.walletPassword = data.user.walletPassword
                this.dataForm.email = data.user.email
                this.dataForm.portrait = data.user.portrait
                this.dataForm.referCode = data.user.referCode
                this.dataForm.status = data.user.status
                this.dataForm.createtime = data.user.createtime
                this.dataForm.updatetime = data.user.updatetime
                this.dataForm.realName = data.user.realName
                this.dataForm.idCard = data.user.idCard
                this.dataForm.secondPassword = data.user.secondPassword
                this.dataForm.alipay = data.user.alipay
                this.dataForm.alipayPic = data.user.alipayPic
                this.dataForm.countryCode = data.user.countryCode
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
              url: this.$http.adornUrl(`/app/user/${!this.dataForm.userId ? 'save' : 'update'}`),
              method: 'post',
              data: this.$http.adornData({
                'userId': this.dataForm.userId || undefined,
                'phone': this.dataForm.phone,
                'appId': this.dataForm.appId,
                'nick': this.dataForm.nick,
                'password': this.dataForm.password,
                'walletPassword': this.dataForm.walletPassword,
                'email': this.dataForm.email,
                'portrait': this.dataForm.portrait,
                'referCode': this.dataForm.referCode,
                'status': this.dataForm.status,
                'createtime': this.dataForm.createtime,
                'updatetime': this.dataForm.updatetime,
                'realName': this.dataForm.realName,
                'idCard': this.dataForm.idCard,
                'secondPassword': this.dataForm.secondPassword,
                'alipay': this.dataForm.alipay,
                'alipayPic': this.dataForm.alipayPic,
                'countryCode': this.dataForm.countryCode
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
