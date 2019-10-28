<template>
  <div class="hello">
    <!--顶部搜索、刷新-->
    <div class="head">
      <el-row>
        <el-col :span="4">
          <el-input v-model="search" placeholder="请输入用户名"></el-input>
        </el-col>
        <el-col :span="2">
          <el-button type="text" @click="onSearch()" class="el-icon-search">查询</el-button>
        </el-col>
        <el-col :span="1">
          <el-button class="el-icon-refresh" type="text" @click="refreshData">刷新</el-button>
        </el-col>
        <el-col :span="1">
          <el-button class="el-icon-circle-plus-outline" type="text" @click="addUser()">添加</el-button>
        </el-col>
      </el-row>
    </div>

    <br>

    <!--表格输入框-->
    <div class="body">
      <el-table :data="tableData" style="width: 100%; text-align: center">

        <el-table-column prop="index" label="序号" width="180">
          <template slot-scope="scope">
            <el-tag :type="scope.row.tag === '家' ? 'primary' : 'success'" disable-transitions>{{scope.$index +1}}
            </el-tag>
          </template>
        </el-table-column>

        <el-table-column prop="userId" label="用户编号" width="180"></el-table-column>

        <el-table-column prop="userName" label="用户名" width="180"></el-table-column>

        <el-table-column prop="sex" label="性别" width="180"></el-table-column>

        <el-table-column prop="phone" label="手机号" width="180"></el-table-column>

        <el-table-column prop="mobile" label="座机号" width="180"></el-table-column>

        <el-table-column prop="degree" label="学历" width="180"></el-table-column>

        <el-table-column prop="deptName" label="部门名称" width="180"></el-table-column>

        <el-table-column prop="duty" label="职务" width="180"></el-table-column>

        <el-table-column prop="mail" label="邮箱" width="180"></el-table-column>

        <el-table-column prop="address" label="地址" width="180"></el-table-column>

        <el-table-column prop="theme" label="主题" width="180"></el-table-column>

        <el-table-column prop="descr" label="描述" width="180"></el-table-column>

        <el-table-column prop="stat" label="状态" width="180"></el-table-column>

        <el-table-column prop="workDate" label="就职日期" width="180" format="yyyy 年 MM 月 dd 日"
                         value-format="yyyy-MM-dd"></el-table-column>

        <el-table-column prop="birthday" label="出生日期" width="180" format="yyyy 年 MM 月 dd 日"
                         value-format="yyyy-MM-dd"></el-table-column>

        <el-table-column prop="handle" label="操作" width="130" fixed="right">
          <template slot-scope="scope">
            <el-button type="primary" icon="el-icon-edit" @click="editUser(scope.row)" circle></el-button>
            <el-button type="danger" icon="el-icon-delete" @click="delUser(scope.row,scope.$index)" circle></el-button>
          </template>
        </el-table-column>
      </el-table>
    </div>

    <br>

    <!--分页操作-->
    <div class="block">
      <el-pagination @size-change="handleSizeChange" @current-change="handleCurrentChange" :current-page="currentPage1"
                     :page-sizes="[5, 10, 20, 50]" :page-size="5"
                     layout="total, sizes, prev, pager, next, jumper" :total="100"></el-pagination>
    </div>

    <!--弹出用户新增表单-->
    <el-dialog title="新增用户" :visible.sync="dialogFormVisible">
      <el-form :model="formData" status-icon label-width="80px" class="add-formData">
        <el-form-item label="用户编号" prop="userId">
          <el-input v-model="formData.userId" autocomplete="off"></el-input>
        </el-form-item>

        <el-form-item label="用户名" prop="userName">
          <el-input v-model="formData.userName" autocomplete="off"></el-input>
        </el-form-item>

        <el-form-item label="密码" prop="password">
          <el-input v-model="formData.password" autocomplete="off"></el-input>
        </el-form-item>

        <el-form-item label="性别" prop="sex">
          <el-input v-model="formData.sex" autocomplete="off"></el-input>
        </el-form-item>

        <el-form-item label="手机号" prop="phone">
          <el-input v-model="formData.phone" autocomplete="off"></el-input>
        </el-form-item>

        <el-form-item label="座机号" prop="mobile">
          <el-input v-model="formData.mobile" autocomplete="off"></el-input>
        </el-form-item>

        <el-form-item label="学历" prop="degree">
          <el-input v-model="formData.degree" autocomplete="off"></el-input>
        </el-form-item>

        <el-form-item label="部门名称" prop="deptName">
          <el-input v-model="formData.deptName" autocomplete="off"></el-input>
        </el-form-item>

        <el-form-item label="职务 " prop="duty">
          <el-input v-model="formData.duty" autocomplete="off"></el-input>
        </el-form-item>

        <el-form-item label="邮箱 " prop="mail">
          <el-input v-model="formData.mail" autocomplete="off"></el-input>
        </el-form-item>

        <el-form-item label="地址 " prop="address">
          <el-input v-model="formData.address" autocomplete="off"></el-input>
        </el-form-item>

        <el-form-item label="主题 " prop="theme">
          <el-input v-model="formData.theme" autocomplete="off"></el-input>
        </el-form-item>

        <el-form-item label="描述 " prop="descr">
          <el-input v-model="formData.descr" autocomplete="off"></el-input>
        </el-form-item>

        <el-form-item label="状态 " prop="stat">
          <el-input v-model="formData.stat" autocomplete="off"></el-input>
        </el-form-item>

        <el-form-item label="就职日期" prop="workDate">
          <el-date-picker v-model="formData.workDate" type="date" placeholder="选择日期" format="yyyy 年 MM 月 dd 日"
                          value-format="yyyy-MM-dd"></el-date-picker>
        </el-form-item>

        <el-form-item label="出生日期" prop="birthday">
          <el-date-picker v-model="formData.birthday" type="date" placeholder="选择日期" format="yyyy 年 MM 月 dd 日"
                          value-format="yyyy-MM-dd"></el-date-picker>
        </el-form-item>

        <el-form-item>
          <el-button @click="dialogFormVisible=false">取 消</el-button>
          <el-button type="primary" @click="addConfirm()">确 定</el-button>
        </el-form-item>
      </el-form>
    </el-dialog>

    <!--弹出用户编辑表单-->
    <el-dialog title="编辑用户" :visible.sync="dialogEditFormVisible">
      <el-form :model="editFormData" status-icon ref="editFormData" label-width="80px" class="add-editFormData">
        <el-form-item label="用户编号" prop="userId">
          <el-input v-model="editFormData.userId" autocomplete="off"></el-input>
        </el-form-item>

        <el-form-item label="用户名" prop="userName">
          <el-input v-model="editFormData.userName" autocomplete="off"></el-input>
        </el-form-item>

        <el-form-item label="密码" prop="password">
          <el-input v-model="editFormData.password" autocomplete="off"></el-input>
        </el-form-item>

        <el-form-item label="性别" prop="sex">
          <el-input v-model="editFormData.sex" autocomplete="off"></el-input>
        </el-form-item>

        <el-form-item label="手机号" prop="phone">
          <el-input v-model="editFormData.phone" autocomplete="off"></el-input>
        </el-form-item>

        <el-form-item label="座机号" prop="mobile">
          <el-input v-model="editFormData.mobile" autocomplete="off"></el-input>
        </el-form-item>

        <el-form-item label="学历" prop="degree">
          <el-input v-model="editFormData.degree" autocomplete="off"></el-input>
        </el-form-item>

        <el-form-item label="部门名称" prop="deptName">
          <el-input v-model="editFormData.deptName" autocomplete="off"></el-input>
        </el-form-item>

        <el-form-item label="职务 " prop="duty">
          <el-input v-model="editFormData.duty" autocomplete="off"></el-input>
        </el-form-item>

        <el-form-item label="邮箱 " prop="mail">
          <el-input v-model="editFormData.mail" autocomplete="off"></el-input>
        </el-form-item>

        <el-form-item label="地址 " prop="address">
          <el-input v-model="editFormData.address" autocomplete="off"></el-input>
        </el-form-item>

        <el-form-item label="主题 " prop="theme">
          <el-input v-model="editFormData.theme" autocomplete="off"></el-input>
        </el-form-item>

        <el-form-item label="描述 " prop="descr">
          <el-input v-model="editFormData.descr" autocomplete="off"></el-input>
        </el-form-item>

        <el-form-item label="状态 " prop="stat">
          <el-input v-model="editFormData.stat" autocomplete="off"></el-input>
        </el-form-item>

        <el-form-item label="就职日期" prop="workDate">
          <el-date-picker v-model="editFormData.workDate" type="date" placeholder="选择日期" format="yyyy 年 MM 月 dd 日"
                          value-format="yyyy-MM-dd"></el-date-picker>
        </el-form-item>

        <el-form-item label="出生日期" prop="birthday">
          <el-date-picker v-model="editFormData.birthday" type="date" placeholder="选择日期" format="yyyy 年 MM 月 dd 日"
                          value-format="yyyy-MM-dd"></el-date-picker>
        </el-form-item>

        <el-form-item>
          <el-button @click="dialogEditFormVisible=false">取 消</el-button>
          <el-button type="primary" @click="editConfirm()">确 定</el-button>
        </el-form-item>
      </el-form>
    </el-dialog>
  </div>
</template>

<script>
    import {error} from "autoprefixer";

    export default {
        name: 'HelloWorld',

        /*数据部分*/
        data() {
            return {    //自己维护的数据
                formData: {  //待添加用户信息
                    userId: '',  //用户编号
                    userName: '', //用户名
                    password: '', //密码
                    sex: '',  //性别
                    phone: '',  //手机号
                    mobile: '',  //座机号
                    birthday: '', //出生日期
                    degree: '',  //学历
                    deptName: '',  //部门名称
                    duty: '',  //职务
                    mail: '',  //邮箱
                    address: '', //地址
                    post: '',  //邮编
                    workDate: '',  //就职日期
                    theme: '', //主题
                    descr: '', //描述
                    stat: '' //状态
                },
                editFormData: {    //待编辑表单数据
                    userId: '',  //用户编号
                    userName: '', //用户名
                    password: '', //密码
                    sex: '',  //性别
                    phone: '',  //手机号
                    mobile: '',  //座机号
                    birthday: '', //出生日期
                    degree: '',  //学历
                    deptName: '',  //部门名称
                    duty: '',  //职务
                    mail: '',  //邮箱
                    address: '', //地址
                    post: '',  //邮编
                    workDate: '',  //就职日期
                    theme: '', //主题
                    descr: '', //描述
                    stat: '' //状态
                },
                searchByUserNameData: [{    //待编辑表单数据
                    userId: '',  //用户编号
                    userName: '', //用户名
                    password: '', //密码
                    sex: '',  //性别
                    phone: '',  //手机号
                    mobile: '',  //座机号
                    birthday: '', //出生日期
                    degree: '',  //学历
                    deptName: '',  //部门名称
                    duty: '',  //职务
                    mail: '',  //邮箱
                    address: '', //地址
                    post: '',  //邮编
                    workDate: '',  //就职日期
                    theme: '', //主题
                    descr: '', //描述
                    stat: '' //状态
                }],
                tableData: [{ //表格中的用户信息
                    userId: '100246',  //用户编号
                    userName: '张三', //用户名
                    sex: '男',  //性别
                    password: '',  //密码
                    phone: '15687678987',  //手机号
                    mobile: '0532-65216782',  //座机号
                    birthday: '2000-01-01', //出生日期
                    degree: '本科',  //学历
                    deptName: '市场部',  //部门名称
                    duty: '销售',  //职务
                    mail: '25457897@qq.com',  //邮箱
                    address: '青岛市南软件园', //地址
                    post: '266042',  //邮编
                    workDate: '2018-01-01',  //就职日期
                    theme: '主题', //主题
                    descr: '个人信息汇总', //描述
                    stat: '在职' //状态
                }],
                pageParam: {
                    pageSizes: 5,    //每页尺寸大小
                    pageCurrent: 1,    //当前页码
                    rowCount: 100,   //总记录数
                },
                currentPage1: 1,   //设定当前页面大小
                dialogTableVisible: false,    //是否显示表格数据
                dialogFormVisible: false,   //是否显示新增表单数据
                dialogEditFormVisible: false,   //是否显示编辑表单数据
                formLabelWidth: '120px',    //新增表单页面宽度
                workValue: '',     //新增表单就职日期数据
                birthdayValue: '',   //新增表单出生日期数据
                search: '',      //绑定查询数据
            }
        },

        /*方法部分*/
        methods: {
            //1.新增用户信息
            addUser() {    //(1).新增按钮
                this.formData = {     //此处很重要，为了避免在新增数据后，数据扔保留，应在点击新增按钮时候做一次清空操作
                    userId: '',  //用户编号
                    userName: '', //用户名
                    password: '', //密码
                    sex: '',  //性别
                    phone: '',  //手机号
                    mobile: '',  //座机号
                    birthday: '', //出生日期
                    degree: '',  //学历
                    deptName: '',  //部门名称
                    duty: '',  //职务
                    mail: '',  //邮箱
                    address: '', //地址
                    post: '',  //邮编
                    workDate: '',  //就职日期
                    theme: '', //主题
                    descr: '', //描述
                    stat: '' //状态
                }

                this.dialogFormVisible = true;    //设置新增表单页面可见性
            },

            addConfirm() {   //(2).新增表单确定按钮

                if (!this.formData.userId) {  //若用户编号未填则给予提示
                    this.$message({
                        message: '用户编号不能为空',
                        type: 'warning'
                    });
                    return; //警告且不返回任何值
                }

                if (!this.formData.userName) {  //若姓名未填则给予提示
                    this.$message({
                        message: '用户名不能为空',
                        type: 'warning'
                    });
                    return; //警告且不返回任何值
                }

                if (!this.formData.sex) {  //若性别未填则给予提示
                    this.$message({
                        message: '性别不能为空',
                        type: 'warning'
                    });
                    return; //警告且不返回任何值
                }

                if (!this.formData.stat) {  //若状态未填则给予提示
                    this.$message({
                        message: '状态不能为空',
                        type: 'warning'
                    });
                    return; //警告且不返回任何值
                }

                //封装数据并将数据传给服务器
                let postDate = this.qs.stringify({
                    userId: this.formData.userId,  //用户编号
                    userName: this.formData.userName, //用户名
                    password: this.formData.password, //密码
                    sex: this.formData.sex,  //性别
                    phone: this.formData.phone,  //手机号
                    mobile: this.formData.mobile,  //座机号
                    birthday: this.formData.birthday, //出生日期
                    degree: this.formData.degree,  //学历
                    deptName: this.formData.deptName,  //部门名称
                    duty: this.formData.duty,  //职务
                    mail: this.formData.mail,  //邮箱
                    address: this.formData.address, //地址
                    post: this.formData.post,  //邮编
                    workDate: this.formData.workDate,  //就职日期
                    theme: this.formData.theme, //主题
                    descr: this.formData.descr, //描述
                    stat: this.formData.stat //状态
                })

                this.$axios({   //将数据发送给后台服务器
                    method: 'post',
                    url: '/user/save',
                    data: postDate
                }).then((response) => {          //这里使用了ES6的语法
                    console.log(response)       //请求成功返回的数据
                }).catch((error) => {
                    console.log(error)       //请求失败返回的数据
                })

                this.tableData.push(this.formData)  //将表单中的数据添加到页面操作
                this.dialogFormVisible = false;    //同时设置新增表单页面不可见
            },

            delUser(item, idx) {    //执行表格操作栏中的删除操作
                this.$confirm('确认删除？')
                    .then(_ => {
                        let delUserId = item.userId;   //先将需要删除的userId保存下来，以便请求后台删除
                        this.tableData.splice(idx, 1);  //删除表格中需要删除的行

                        this.axios({    //向后台发送删除数据请求，并将结果打印到控制台
                            method: 'post',
                            url: '/user/delete',
                            data: {userId: delUserId}
                        }).then(response => {
                            console.log(response);  //可以接受控制台打印的数据(实际情况下则不需要)
                            window.location.reload();
                        }).catch(error => {
                            console.log(error);
                        })

                    }).catch(_ => {
                    console.log(error);   //此处不做任何操作即可
                });
            },

            editUser(item) {   //执行表格操作栏编辑操作从后台取数据（不从页面拿数据，避免脏读）
                this.axios({    //向后台发送请求，根据userId获取数据
                    method: 'get',
                    url: '/user/findEditByUserId',
                    data: {userId: item.userId}
                }).then(response => {
                    console.log(response);  //将后台获取的数据先打印到后台再回写到编辑页面
                    this.editFormData = {    //待编辑表单数据
                        userId: response.data.userId,  //用户编号
                        userName: response.data.userName, //用户名
                        password: response.data.password, //密码
                        sex: response.data.sex,  //性别
                        phone: response.data.phone,  //手机号
                        mobile: response.data.mobile,  //座机号
                        birthday: response.data.birthday, //出生日期
                        degree: response.data.degree,  //学历
                        deptName: response.data.deptName,  //部门名称
                        duty: response.data.duty,  //职务
                        mail: response.data.mail,  //邮箱
                        address: response.data.address, //地址
                        post: response.data.post,  //邮编
                        workDate: response.data.workDate,  //就职日期
                        theme: response.data.theme, //主题
                        descr: response.data.descr, //描述
                        stat: response.data.stat //状态
                    }
                }).catch(error => {
                    console.log(error);
                })

                this.dialogEditFormVisible = true;    //点击编辑按钮，执行页面显示操作
            },

            editConfirm() {    //执行编辑菜单确定按钮,首先将结果写到页面，之后向后台发送请求
                if (!this.editFormData.userId) {  //若用户编号未填则给予提示
                    this.$message({
                        message: '用户编号不能为空',
                        type: 'warning'
                    });
                    return; //警告且不返回任何值
                }

                if (!this.editFormData.userName) {  //若姓名未填则给予提示
                    this.$message({
                        message: '用户名不能为空',
                        type: 'warning'
                    });
                    return; //警告且不返回任何值
                }

                if (!this.editFormData.sex) {  //若性别未填则给予提示
                    this.$message({
                        message: '性别不能为空',
                        type: 'warning'
                    });
                    return; //警告且不返回任何值
                }

                if (!this.editFormData.stat) {  //若状态未填则给予提示
                    this.$message({
                        message: '状态不能为空',
                        type: 'warning'
                    });
                    return; //警告且不返回任何值
                }

                this.tableData = {    //将表单中修改的数据回写到页面
                    userId: this.editFormData.userId,  //用户编号
                    userName: this.editFormData.userName, //用户名
                    password: this.editFormData.password, //密码
                    sex: this.editFormData.sex,  //性别
                    phone: this.editFormData.phone,  //手机号
                    mobile: this.editFormData.mobile,  //座机号
                    birthday: this.editFormData.birthday, //出生日期
                    degree: this.editFormData.degree,  //学历
                    deptName: this.editFormData.deptName,  //部门名称
                    duty: this.editFormData.duty,  //职务
                    mail: this.editFormData.mail,  //邮箱
                    address: this.editFormData.address, //地址
                    post: this.editFormData.post,  //邮编
                    workDate: this.editFormData.workDate,  //就职日期
                    theme: this.editFormData.theme, //主题
                    descr: this.editFormData.descr, //描述
                    stat: this.editFormData.stat //状态
                }

                this.dialogEditFormVisible = false;    //同时设置新增表单页面不可见

                //封装数据并将数据传给服务器
                let editResult = this.qs.stringify({
                    userId: this.editFormData.userId,  //用户编号
                    userName: this.editFormData.userName, //用户名
                    password: this.editFormData.password, //密码
                    sex: this.editFormData.sex,  //性别
                    phone: this.editFormData.phone,  //手机号
                    mobile: this.editFormData.mobile,  //座机号
                    birthday: this.editFormData.birthday, //出生日期
                    degree: this.editFormData.degree,  //学历
                    deptName: this.editFormData.deptName,  //部门名称
                    duty: this.editFormData.duty,  //职务
                    mail: this.editFormData.mail,  //邮箱
                    address: this.editFormData.address, //地址
                    post: this.editFormData.post,  //邮编
                    workDate: this.editFormData.workDate,  //就职日期
                    theme: this.editFormData.theme, //主题
                    descr: this.editFormData.descr, //描述
                    stat: this.editFormData.stat //状态
                })

                this.$axios({   //将编辑完成的数据发送给后台服务器
                    method: 'post',
                    url: '/user/editConfirm',
                    data: editResult
                }).then((response) => {          //这里使用了ES6的语法
                    console.log(response)       //请求成功返回的数据
                }).catch((error) => {
                    console.log(error)       //请求失败返回的数据
                })
            },

            onSearch() {   //执行userName参数查询操作(从数据库中查询)，并将结果显示到页面
                let searchByUserName = this.search;   //输入框中输入的数据

                this.$axios({   //将数据发送给后台服务器,执行查询操作，之后将结果展示到页面
                    method: 'get',
                    url: '/user/searchByParam',
                    data: searchByUserName
                }).then((response) => {          //这里使用了ES6的语法
                    console.log(response)       //控制台打印请求成功返回的数据
                    tableData: [{ //表格中的用户信息(注意：将查询结果回写到页面之前，首先将表格中的数据清空)
                        userId: '',  //用户编号
                        userName: '', //用户名
                        sex: '',  //性别
                        password: '',  //密码
                        phone: '',  //手机号
                        mobile: '',  //座机号
                        birthday: '', //出生日期
                        degree: '',  //学历
                        deptName: '',  //部门名称
                        duty: '',  //职务
                        mail: '',  //邮箱
                        address: '', //地址
                        post: '',  //邮编
                        workDate: '',  //就职日期
                        theme: '', //主题
                        descr: '', //描述
                        stat: '' //状态
                    }]
                    this.tableData=response.data;   //将后台数据库中查询出来的数据全部赋值到表格中
                }).catch((error) => {
                    console.log(error)       //请求失败返回的数据
                })
            },

            //执行刷新reload操作
            refreshData() {
                location.reload();
            },


            //下面两个方法暂不处理
            handleSizeChange() {   //分页查询事件中，触发页面大小改变事件
                let handlePageSize = pageSize;    //指定每页大小
                this.$axios({   //将数据发送给后台服务器,执行查询操作，之后将结果展示到页面
                    method: 'post',
                    url: '/user/searchByPageSize',
                    data: handlePageSize
                }).then((response) => {          //这里使用了ES6的语法
                    console.log(response)       //控制台打印请求成功返回的数据
                    this.tableData.push(this.formData)  //将表单中的数据添加到页面操作
                }).catch((error) => {
                    console.log(error)       //请求失败返回的数据
                })
            },
            handleCurrentChange(val) {    //分页查询事件中，触发当前页改变事件

                console.log(`当前页: ${val}`);
            }

        }

    }
</script>

<style scoped>

</style>
