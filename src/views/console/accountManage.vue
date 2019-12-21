<template>
  <div class="app-container">

    <el-form ref="ruleForm" :model="ruleForm" :inline="true" label-width="100px" class="demo-ruleForm">
      <el-form-item label="账号/昵称">
        <el-input v-model="ruleForm.userName" placeholder="账号/昵称" />
      </el-form-item>

      <el-form-item>
        <el-button type="primary" @click="submitForm('ruleForm')">查询</el-button>
      </el-form-item>

      <el-form-item>
        <el-button type="primary" @click="dialogFormVisible = true">添加</el-button>
      </el-form-item>
    </el-form>

    <el-table v-loading="listLoading" :data="list" border fit highlight-current-row style="width: 100%">
      <el-table-column align="center" label="青群用户" min-width="300px">
        <template slot-scope="">
          <span>1111</span>
        </template>
      </el-table-column>

      <el-table-column width="180px" align="center" label="账号">
        <template slot-scope="scope">
          <span>1111</span>
        </template>
      </el-table-column>

      <el-table-column width="180px" align="center" label="姓名">
        <template slot-scope="scope">
          <span>1111</span>
        </template>
      </el-table-column>

      <el-table-column width="180px" align="center" label="权限">
        <template slot-scope="scope">
          <svg-icon v-for="n in +scope.row.importance" :key="n" icon-class="star" class="meta-item__icon" />
        </template>
      </el-table-column>

      <el-table-column class-name="status-col" align="center" label="状态" width="180px">
        <template slot-scope="">
          <span>1111</span>
        </template>
      </el-table-column>

      <el-table-column width="400px" align="center" label="创建时间">
        <el-button type="primary" size="small" @click="dialogVisible = true">
          启用
        </el-button>
        <el-button type="primary" size="small">
          重置密码
        </el-button>
      </el-table-column>
    </el-table>

    <el-dialog
      title="提示"
      :visible.sync="dialogVisible"
      width="30%"
      :before-close="handleClose"
    >
      <span>这是一段信息</span>
      <span slot="footer" class="dialog-footer">
        <el-button @click="dialogVisible = false">取 消</el-button>
        <el-button type="primary" @click="confirmFunc()">确 定</el-button>
      </span>
    </el-dialog>

    <el-dialog title="添加账户" :visible.sync="dialogFormVisible">
      <el-form ref="form" :model="form" :rules="rules">
        <el-form-item label="账号" :label-width="formLabelWidth" prop="account">
          <el-input v-model="form.account" placeholder="请输入4-12位的账号" autocomplete="off" />
        </el-form-item>
        <el-form-item label="姓名" :label-width="formLabelWidth" prop="name">
          <el-input v-model="form.name" autocomplete="off" placeholder="请输入姓名" />
        </el-form-item>
        <el-form-item label="密码" :label-width="formLabelWidth" prop="password">
          <el-input v-model="form.password" autocomplete="off" placeholder="默认123456" />
        </el-form-item>
        <el-form-item label="权限" :label-width="formLabelWidth">
          <el-select v-model="form.authority" placeholder="管理员" prop="authority" required>
            <el-option label="管理员" value="shanghai" />
          </el-select>
        </el-form-item>
      </el-form>
      <div slot="footer" class="dialog-footer">
        <el-button @click="dialogFormVisible = false">取 消</el-button>
        <el-button type="primary" @click="submitAccount('form')">确 定</el-button>
      </div>
    </el-dialog>

    <pagination v-show="total>0" :total="total" :page.sync="listQuery.page" :limit.sync="listQuery.limit" @pagination="getList" />
  </div>
</template>

<script>
import { fetchList } from '@/api/article'
import Pagination from '@/components/Pagination' // Secondary package based on el-pagination

export default {
  name: 'ArticleList',
  components: {
    Pagination
  },
  filters: {
    statusFilter(status) {
      const statusMap = {
        published: 'success',
        draft: 'info',
        deleted: 'danger'
      }
      return statusMap[status]
    }
  },
  data() {
    return {
      dialogVisible: false, // 是否显示对话框
      list: null,
      total: 0,
      listLoading: true,
      listQuery: {
        page: 1,
        limit: 20
      },
      ruleForm: {
        accountName: '',
        userName: '',
        date1: ''
      },

      dialogFormVisible: false, // 是否显示添加账户弹层
      form: {
        account: '',
        name: '',
        password: '123456',
        authority: '管理员'
      },
      rules: {
        account: [
          { required: true, message: '请输入4-12位的账号', trigger: 'blur' },
          { min: 4, max: 12, message: '长度在 4 到 12 个字符', trigger: 'blur' }
        ],
        name: [
          { required: true, message: '请输入4-12位的姓名', trigger: 'blur' },
          { min: 4, max: 12, message: '长度在 4 到 12 个字符', trigger: 'blur' }
        ],
        password: [
          { required: true, message: '请输入4-12位的姓名', trigger: 'blur' },
          { min: 4, max: 12, message: '长度在 4 到 12 个字符', trigger: 'blur' }
        ],
        authority: [
          { required: true, message: '请选择权限', trigger: 'change' }
        ]
      },
      formLabelWidth: '120px'
    }
  },
  created() {
    this.getList()
  },
  methods: {
    getList() {
      this.listLoading = true
      fetchList(this.listQuery).then(response => {
        this.list = response.data.items
        this.total = response.data.total
        this.listLoading = false
      })
    },

    submitForm(formName) {
      this.$refs[formName].validate((valid) => {
        if (valid) {
          alert('submit!')
        } else {
          console.log('error submit!!')
          return false
        }
      })
    },
    resetForm(formName) {
      this.$refs[formName].resetFields()
    },

    // 关闭提示框
    handleClose(done) {
      this.$confirm('确认关闭？')
        .then(_ => {
          done()
        })
        .catch(_ => {})
    },

    // 关闭提示框
    confirmFunc() {
      this.dialogVisible = !this.dialogVisible
    },

    // 提交账户信息
    submitAccount(formName) {
      const that = this
      this.$refs[formName].validate((valid) => {
        if (valid) {
          that.dialogFormVisible = !that.dialogFormVisible
          that.$message({
            message: '提交成功',
            type: 'success'
          })
        } else {
          console.log('error submit!!')
          return false
        }
      })
    }
  }
}
</script>

<style scoped>
.edit-input {
  padding-right: 100px;
}
.cancel-btn {
  position: absolute;
  right: 15px;
  top: 10px;
}
</style>
