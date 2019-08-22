<template>
  <el-dialog
    :title="!dataForm.id ? '新增' : '修改'"
    :close-on-click-modal="false"
    :visible.sync="visible">
    <el-form :model="dataForm" :rules="dataRule" ref="dataForm" @keyup.enter.native="dataFormSubmit()" label-width="80px">
    <el-form-item label="key" prop="key">
      <el-input v-model="dataForm.key" placeholder="key"></el-input>
    </el-form-item>
    <el-form-item label="text-value" prop="value">
      <el-input v-model="dataForm.value" placeholder="text-value"></el-input>
    </el-form-item>
    <el-form-item label="变量类型" prop="type">
      <el-input v-model="dataForm.type" placeholder="变量类型"></el-input>
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
          key: '',
          value: '',
          type: '',
          createtime: '',
          updatetime: ''
        },
        dataRule: {
          key: [
            { required: true, message: 'key不能为空', trigger: 'blur' }
          ]
          // value: [
          //   { required: true, message: 'text-value不能为空', trigger: 'blur' }
          // ],
          // type: [
          //   { required: true, message: '变量类型不能为空', trigger: 'blur' }
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
              url: this.$http.adornUrl(`/app/systemdata/info/${this.dataForm.id}`),
              method: 'get',
              params: this.$http.adornParams()
            }).then(({data}) => {
              if (data && data.code === 0) {
                this.dataForm.key = data.systemdata.key
                this.dataForm.value = data.systemdata.value
                this.dataForm.type = data.systemdata.type
                this.dataForm.createtime = data.systemdata.createtime
                this.dataForm.updatetime = data.systemdata.updatetime
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
              url: this.$http.adornUrl(`/app/systemdata/${!this.dataForm.id ? 'save' : 'update'}`),
              method: 'post',
              data: this.$http.adornData({
                'id': this.dataForm.id || undefined,
                'key': this.dataForm.key,
                'value': this.dataForm.value,
                'type': this.dataForm.type,
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
