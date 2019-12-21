<template>
  <div class="app-container">

    <el-form ref="ruleForm" :model="ruleForm" :inline="true" label-width="100px" class="demo-ruleForm">
      <el-form-item label="业务状态">
        <el-select v-model="ruleForm.state" placeholder="全部状态">
          <el-option label="全部状态" value="shanghai" />
          <el-option label="未开通" value="beijing" />
          <el-option label="正常" value="beijing" />
          <el-option label="暂停" value="beijing" />
        </el-select>
      </el-form-item>
      <el-form-item label="注册时间">
        <el-col :span="11">
          <el-form-item>
            <el-date-picker v-model="ruleForm.date1" type="date" placeholder="开始时间" style="width: 100%;" />
          </el-form-item>
        </el-col>
        <el-col class="line" :span="2">-</el-col>
        <el-col :span="11">
          <el-form-item>
            <el-date-picker v-model="ruleForm.date2" placeholder="结束时间" style="width: 100%;" />
          </el-form-item>
        </el-col>
      </el-form-item>
      <el-form-item label="绑定群主">
        <el-input v-model="ruleForm.accountName" placeholder="请输入青苹果账户名" />
      </el-form-item>

      <el-form-item label="青群用户">
        <el-input v-model="ruleForm.userName" placeholder="请输入用户名" />
      </el-form-item>
      <el-form-item>
        <el-button type="primary" @click="submitForm('ruleForm')">查询</el-button>
      </el-form-item>
    </el-form>

    <el-table v-loading="listLoading" :data="list" border fit highlight-current-row style="width: 100%">
      <el-table-column align="center" label="青群用户" width="80">
        <template slot-scope="scope">
          <span>{{ scope.row.id }}</span>
        </template>
      </el-table-column>

      <el-table-column width="180px" align="center" label="绑定群主">
        <template slot-scope="scope">
          <span>11111</span>
        </template>
      </el-table-column>

      <el-table-column width="120px" align="center" label="联系人">
        <template slot-scope="scope">
          <span>{{ scope.row.author }}</span>
        </template>
      </el-table-column>

      <el-table-column align="center" width="100px" label="手机号">
        <template slot-scope="scope">
          <svg-icon v-for="n in +scope.row.importance" :key="n" icon-class="star" class="meta-item__icon" />
        </template>
      </el-table-column>

      <el-table-column align="center" class-name="status-col" label="注册时间" width="300px">
        <template slot-scope="scope">
          <span>{{ scope.row.timestamp | parseTime('{y}-{m}-{d} {h}:{i}') }}</span>
        </template>
      </el-table-column>

      <el-table-column align="center" width="300px" label="业务状态">
        <template slot-scope="{row}">
          <span>{{ row.title }}</span>
        </template>
      </el-table-column>

      <el-table-column align="center" min-width="300px" label="持续时间">
        <template slot-scope="{row}">
          <span>{{ row.title }}</span>
        </template>
      </el-table-column>

      <el-table-column align="center" label="操作" width="200px">
        <template slot-scope="scope">
          <el-button type="primary" size="small" @click="dialogFormVisible = true">
            设置业务
          </el-button>
        </template>
      </el-table-column>
    </el-table>

    <pagination v-show="total>0" :total="total" :page.sync="listQuery.page" :limit.sync="listQuery.limit" @pagination="getList" />

    <el-dialog title="开通账户" width="20%" :visible.sync="dialogFormVisible">
      <el-form ref="form" :model="form">
        <el-form-item>
          <el-date-picker v-model="date" placeholder="截止时间" type="datetime" style="width: 100%;" />
        </el-form-item>
      </el-form>
      <div slot="footer" class="dialog-footer">
        <el-button @click="dialogFormVisible = false">取 消</el-button>
        <el-button type="primary" @click="confirmFunc('form')">确 定</el-button>
      </div>
    </el-dialog>
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
      date: '',
      dialogFormVisible: false,
      list: null,
      total: 0,
      listLoading: true,
      listQuery: {
        page: 1,
        limit: 20
      },
      form: {

      },
      ruleForm: {
        accountName: '',
        userName: '',
        state: '',
        date1: '',
        date2: ''
      }
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
      this.dialogFormVisible = !this.dialogFormVisible
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
