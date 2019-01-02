<template>
  <el-card class="box-card" shadow="always">
    <el-breadcrumb separator="/">
      <el-breadcrumb-item :to="{ path: '/' }">首页</el-breadcrumb-item>
      <el-breadcrumb-item>用户管理</el-breadcrumb-item>
      <el-breadcrumb-item>用户列表</el-breadcrumb-item>
    </el-breadcrumb>
    <!-- 搜索框 -->
    <el-row>
      <el-col>
        <el-input placeholder="请输入内容" v-model="query" class="input-with-select" clearable @clear='getAlluser'>
          <el-button slot="append" icon="el-icon-search" @click='serachUser()'></el-button>
        </el-input>
        <el-button type="primary" @click='showUsersDialog()'>添加用户</el-button>
      </el-col>
    </el-row>

    <!-- 表格组件 -->
    <el-table height="300px" :data="tableData" style="width: 100%">
      <el-table-column prop="id" label="#" width="80">
      </el-table-column>
      <el-table-column prop="username" label="姓名" width="100">
      </el-table-column>
      <el-table-column prop="email" label="邮箱" width="180">
      </el-table-column>
      <el-table-column prop="mobile" label="电话" width="180">
      </el-table-column>
      <el-table-column label="创建日期" width="180">
        <template slot-scope="scope">
          {{scope.row.create_time | fmtDate}}
        </template>
      </el-table-column>
      <el-table-column prop="mg_state" label="用户状态" width="80">
        <template slot-scope="scope">
          <el-switch active-color="#13ce66" inactive-color="#ff4949" v-model="scope.row.mg_state" @change='changeStatusUser(scope.row)'>
          </el-switch>
        </template>
      </el-table-column>
      <el-table-column prop="edit" label="操作">
        <template slot-scope="scope">
          <el-button size="mini" plain type="primary" icon="el-icon-edit" @click='showEditUsersDialog(scope.row)' circle></el-button>
          <el-button size="mini" plain type="danger" icon="el-icon-delete" @click='delUser(scope.row)' circle></el-button>
          <el-button size="mini" plain type="success" icon="el-icon-check" @click='showUsersRows(scope.row)' circle></el-button>
        </template>
      </el-table-column>
    </el-table>

    <!-- 分页组件 -->
    <el-pagination @size-change="handleSizeChange" @current-change="handleCurrentChange" :current-page="pagenum" :page-sizes="[2, 4, 6, 8]"
      :page-size="2" layout="total, sizes, prev, pager, next, jumper" :total="total">
    </el-pagination>

    <!--添加用户 对话框 -->
    <el-dialog title="添加用户信息" :visible.sync="dialogFormVisibleAdd">
      <el-form :model="form">
        <el-form-item label="用户名" :label-width="formLabelWidth">
          <el-input v-model="form.username" autocomplete="off"></el-input>
        </el-form-item>
        <el-form-item label="密码" :label-width="formLabelWidth">
          <el-input v-model="form.password" autocomplete="off"></el-input>
        </el-form-item>
        <el-form-item label="邮箱" :label-width="formLabelWidth">
          <el-input v-model="form.email" autocomplete="off"></el-input>
        </el-form-item>
        <el-form-item label="电话" :label-width="formLabelWidth">
          <el-input v-model="form.mobile" autocomplete="off"></el-input>
        </el-form-item>
      </el-form>
      <div slot="footer" class="dialog-footer">
        <el-button @click="dialogFormVisibleAdd = false">取 消</el-button>
        <el-button type="primary" @click="addUser()">确 定</el-button>
      </div>
    </el-dialog>

    <!-- 编辑对话框 -->
    <el-dialog title="修改用户信息" :visible.sync="dialogFormVisibleEdit">
      <el-form :model="form">
        <el-form-item label="用户名" :label-width="formLabelWidth">
          <el-input disabled v-model="form.username" autocomplete="off"></el-input>
        </el-form-item>
        <el-form-item label="邮箱" :label-width="formLabelWidth">
          <el-input v-model="form.email" autocomplete="off"></el-input>
        </el-form-item>
        <el-form-item label="电话" :label-width="formLabelWidth">
          <el-input v-model="form.mobile" autocomplete="off"></el-input>
        </el-form-item>
      </el-form>
      <div slot="footer" class="dialog-footer">
        <el-button @click="dialogFormVisibleEdit = false">取 消</el-button>
        <el-button type="primary" @click="editUser()">确 定</el-button>
      </div>
    </el-dialog>

    <!-- 分配角色对话框 -->
    <el-dialog title="分配角色" :visible.sync="setRoleDialogFormVisible">
      <el-form label-width="100px" :model="form">
        <el-form-item label="用户名" prop="username">
          <el-input disabled auto-complete="off" v-model='form.username'></el-input>
        </el-form-item>
        <el-form-item label="角色">
          <el-select>
            <el-option disabled label="请选择" value="-1">
            </el-option>
          </el-select>
        </el-form-item>
      </el-form>
      <div slot="footer" class="dialog-footer">
        <el-button @click="setRoleDialogFormVisible = false">取 消</el-button>
        <el-button type="primary">确 定</el-button>
      </div>
    </el-dialog>

  </el-card>
</template>

<script>
  export default {

    data() {
      return {
        tableData: [],
        query: '',
        pagenum: 1,
        pagesize: 2,
        query: '',
        total: -1,
        dialogFormVisibleAdd: false,
        dialogFormVisibleEdit: false,
        setRoleDialogFormVisible:false,
        formLabelWidth: '100px',
        form: {
          username: '',
          password: '',
          email: '',
          mobile: ''
        }
      }
    },
    created() {
      this.getUser()
    },
    methods: {

      // 显示用户角色的对话框
    async  showUsersRows(user) {
        this.setRoleDialogFormVisible=true;
        // this.showEditUsersDialog(user);
        const res = await this.$http.put(`users/${user.id}/role`)
      },

      // 修改用户的状态
      async changeStatusUser(user) {
        const res = await this.$http.put(`users/${user.id}/state/${user.mg_state}`);
        console.log(res);
        const { data: { meta: { msg, status } } } = res;
        if (status === 200) {
          this.$message.success(msg);
          this.getUser()
        }
      },

      // 显示编辑对话框
      async showEditUsersDialog(user) {
        this.dialogFormVisibleEdit = true;
        const res = await this.$http.get(`users/${user.id}`);
        // console.log(res);
        const { data, meta: { msg, status } } = res.data;
        if (status === 200) {
          this.form = data;
        }
      },
      // 修改时的方法
      async editUser() {
        const res = await this.$http.put(`users/${this.form.id}`, {
          email: this.form.email,
          mobile: this.form.mobile
        });
        // console.log(res);
        const { data: { meta: { msg, status } } } = res;
        if (status === 200) {
          this.$message.success(msg);
          this.dialogFormVisibleEdit = false;
          this.getUser();
        }

      },
      // 显示删除对话框
      delUser(user) {
        this.$confirm('您确定要删除此用户吗?', '提示', {
          confirmButtonText: '确定',
          cancelButtonText: '取消',
          type: 'warning'
        }).then(async () => {
          const res = await this.$http.delete(`users/${user.id}`);
          console.log(res);
          const { data: { meta: { msg, status } } } = res;

          if (status === 200) {

            this.$message.success('删除用户成功');
            this.getUser();
            this.pagenum = 1;
          }
        }).catch(() => {
          this.$message.warning('已取消删除该用户');
        });
      },

      // 添加用户信息
      async addUser() {
        const res = await this.$http.post(`users`, this.form);
        console.log(res);
        const { data: { meta: { status, msg } } } = res;
        if (status === 201) {
          this.$message.success('添加用户成功');
          this.dialogFormVisibleAdd = false;
          this.getUser();
        } else {
          this.$message.warning(msg);
        }
      },
      // 显示添加用户对话框
      showUsersDialog() {
        this.form = {};
        this.dialogFormVisibleAdd = true;

      },

      // 显示所有用户
      getAlluser() {
        this.getUser()
      },
      // 搜索用户
      serachUser() {
        this.pagenum = 1;
        this.getUser();
      },
      // 分页
      handleSizeChange(val) {
        console.log(`每页 ${val} 条`);
        this.pagesize = val;
        this.pagenum = 1;
        this.getUser()
      },
      handleCurrentChange(val) {
        console.log(`当前页: ${val}`);
        this.pagenum = val;
        this.getUser();
      },

      async  getUser() {
        const AUTH_TOKEN = localStorage.getItem('token')
        this.$http.defaults.headers.common['Authorization'] = AUTH_TOKEN
        const res = await this.$http.get(`users?query=${this.query}&pagenum=${this.pagenum}&pagesize=${this.pagesize}`);
        console.log(res);
        const {
          data: { total, users }, meta: { status, msg }
        } = res.data;
        if (status === 200) {
          this.tableData = users;
          this.total = total;
        }

      }
    }

  }
</script>

<style>
  .box-card {
    height: 100%
  }

  .input-with-select {
    margin-top: 20px;
    width: 350px;
  }
</style>