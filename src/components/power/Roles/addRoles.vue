<template>
  <div>
    <el-card>
      <el-page-header @back="goBack" content="添加角色"> </el-page-header>
      <el-form
        ref="roleFormRef"
        :rules="roleFormRules"
        :model="roleForm"
        label-width="80px"
      >
        <el-form-item label="角色名称" prop="roleName">
          <el-input v-model="roleForm.roleName"></el-input>
        </el-form-item>
        <el-form-item label="角色描述">
          <el-input v-model="roleForm.roleDesc"></el-input>
        </el-form-item>
        <el-form-item>
          <el-button type="primary" @click="addRole">立即创建</el-button>
          <el-button @click="goBack">取消</el-button>
        </el-form-item>
      </el-form>
    </el-card>
  </div>
</template>

<script>
export default {
  data() {
    return {
      roleForm: {
        roleName: '',
        roleDesc: ''
      },
      roleFormRules: {
        roleName: [
          { required: true, message: '请输入角色名称', trigger: 'blur' }
        ]
      }
    }
  },
  methods: {
    goBack() {
      this.$router.go(-1)
    },
    addRole() {
      this.$refs.roleFormRef.validate(async (valid) => {
        if (!valid) return
        const { data: res } = await this.$http.post('roles', this.roleForm)
        if (res.meta.status !== 201) {
          return this.$message.error('添加角色失败！')
        }
        this.$message.success('成功添加角色！')
        this.$router.go(-1)
      })
    }
  }
}
</script>

<style lang="scss" scoped>
.el-page-header {
  margin-bottom: 1.25rem;
}
</style>
