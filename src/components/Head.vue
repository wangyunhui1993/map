<template>
    <el-row class="head bg-blue">
        <el-col :span="24" style="height:100%">
            <div class="net-title">
                智能柜管理系统
            </div>
            <div class="exit">
                <el-dropdown trigger="click">
                    <span class="el-dropdown-link" style="color: #fff;cursor: pointer;">
                        设置<i class="el-icon-arrow-down el-icon--right"></i>
                    </span>
                    <el-dropdown-menu slot="dropdown" style="width: 120px;">
                        <el-dropdown-item>
                            <p @click="personalInfoStatus = true">个人资料</p>
                        </el-dropdown-item>
                        <el-dropdown-item>
                            <p @click="updatePasswordStatus = true">修改密码</p>
                        </el-dropdown-item>
                        <el-dropdown-item>
                            <p @click="quit">退出</p>
                        </el-dropdown-item>
                    </el-dropdown-menu>
                </el-dropdown>
            </div>
            <div class="login-info">
                <span class="welcome">欢迎您</span>
                <span>{{account.name}}</span>
            </div>
            <div class="now-time">
                <span>{{nowTime}}</span>
            </div>
        </el-col>
        <!-- 个人资料模态框 -->
        <el-dialog title="个人资料" :visible.sync="personalInfoStatus" width="400px">
            <div class="line">
                <span class="label">用户名：</span>
                <span>{{account.username}}</span>
            </div>
            <div class="line">
                <span class="label">姓名：</span>
                <span>{{account.name}}</span>
            </div>
            <div class="line">
                <span class="label">角色：</span>
                <span>{{account.role}}</span>
            </div>
            <div class="btn_wrap">
                <el-button type="primary" @click="personalInfoStatus = false">确定</el-button>
            </div>
        </el-dialog>

        <!-- 修改密码模态框 -->
        <el-dialog title="个人资料" :visible.sync="updatePasswordStatus" width="600px" @close="modelClose">
            <el-form :model="passwordInfo"  label-width="80px" status-icon :rules="rules" ref="ruleForm">
                <el-form-item label="原密码" prop="password">
                    <el-input type="password" v-model="passwordInfo.password" placeholder="请输入原密码"></el-input>
                </el-form-item>
                <el-form-item label="新密码" prop="newPassword">
                    <el-input type="password" v-model="passwordInfo.newPassword" placeholder="请输入新密码"></el-input>
                </el-form-item>
                <el-form-item label="确认密码" prop="checkPassword">
                    <el-input type="password" v-model="passwordInfo.checkPassword" placeholder="请再次输入密码"></el-input>
                </el-form-item>
            </el-form>
            <div class="btn_wrap">
                <el-button type="primary" @click="submitUpdatePassword">保存</el-button>
            </div>
        </el-dialog>
    </el-row>
</template>
<script>
    // import { UpdatePassword } from '@/js/api';
    export default{
        data(){
            var validatePass = (rule, value, callback) => {
                    if (value === '') {
                        callback(new Error('请输入新密码'));
                    } else {
                        if (this.passwordInfo.checkPassword !== '') {
                            this.$refs.ruleForm.validateField('checkPassword');
                        }
                        callback();
                    }
                };
            var validatePass2 = (rule, value, callback) => {
                    if (value === '') {
                        callback(new Error('请再次输入密码'));
                    } else if (value !== this.passwordInfo.newPassword) {
                        callback(new Error('两次输入密码不一致!'));
                    } else {
                        callback();
                    }
                };
            return{
                nowTime: '',
                personalInfoStatus: false,
                updatePasswordStatus: false,
                passwordInfo: {
                    password: '',
                    newPassword: '',
                    checkPassword: ''
                },
                rules: {
                    password: [
                        { required: true, message: '请输入原密码', trigger: 'blur' },
                    ],
                    newPassword: [
                        { required: true, message: '请输入新密码', trigger: 'blur' },
                        { validator: validatePass, trigger: 'blur' }
                    ],
                    checkPassword: [
                        { required: true, message: '请再次输入密码', trigger: 'blur' },
                        { validator: validatePass2, trigger: 'blur' }
                    ],
                    
                }
            }
        },
        computed: {
            account() {
                // return this.$store.state.account;
                return {
                    username: '1001',
                    name: '张三',
                    role: '系统管理员'
                }
            }
        },
        methods:{
            /*修改密码*/
            submitUpdatePassword(){
                this.$refs['ruleForm'].validate((valid) => {
                    if (valid) {
                        let params = {};
                        params.adminNumber = 0;
                        params.updateInfo = {
                            id: this.account.id,
                            originalPwd: this.passwordInfo.password,
                            password: this.passwordInfo.newPassword
                        }
                        UpdatePassword(params).then(data => {
                            let {errMsg, errCode, value, extralInfo, success} = data;
                            if(success) {
                                this.updatePasswordStatus = false;
                                this.$message({
                                    message: '修改成功',
                                    type: 'success'
                                });
                            } else {
                                this.$message({
                                    message: errMsg,
                                    type: 'error'
                                });
                            };
                        });
                    } else {
                        return false;
                    }
                });
            },
            //关闭修改密码模态框
            modelClose(){
                this.$refs['ruleForm'].resetFields();
            },
            //退出
            quit(){
                this.$confirm('确认退出吗?', '提示', {
                    type: 'info'
                }).then(() => {
                    this.$router.push('/login');
                }).catch(() => {

                });
            },
            showTime(){
                var date = new Date();
                var year = date.getFullYear();
                var month = date.getMonth()+1;
                if(month <= 9){
                    month = "0" + month;
                }
                var day = date.getDate();
                if(day <= 9){
                    day = "0" + day;
                }
                var hours = date.getHours();
                if(hours <= 9){
                    hours = "0" + hours;
                }
                var minutes = date.getMinutes();
                if(minutes <= 9){
                    minutes = "0" + minutes;
                }
                var seconds = date.getSeconds();
                if(seconds <= 9){
                    seconds = "0" + seconds;    
                }
                this.nowTime = year + '-' + month + '-' + day + ' ' + hours + ':' + minutes + ':' + seconds;
            }
        },
        mounted() {
            this.showTime();
            setInterval(this.showTime,1000);
        }
    }
</script>
<style scoped>
    .head{
        min-height: 60px;
        line-height:60px;
    }
    .bg-blue{
        background: #1D8CE0;
    }
    .net-title{
        float:left;
        margin-left:20px;
        height:100%;
        color:#fff;
        font-weight: bold;
        font-size:18px;
    }
    .welcome{
        color:#fff;
        font-style: italic;
    }
    .login-info{
        float:right;
        height:100%;
        margin-right:20px;
        font-size:18px;
    }
    .exit{
        float:right;
        margin-right:50px;
        color:#fff;
        font-size:14px;
    }
    .now-time{
        float:right;
        margin-right:20px;
        color:#fff;
    }
    .turn{
        transform:rotate(90deg);
        -ms-transform:rotate(90deg);
        -moz-transform:rotate(90deg);
        -webkit-transform:rotate(90deg);
        -o-transform:rotate(90deg);
    }
    a{
        text-decoration: none;
        color:white;
    }
    .line{
        font-size: 16px;
        line-height: 40px;
    }
    .label{
        display: inline-block;
        width: 140px;
        text-align: right;
    }
    .label + span{
        padding-left: 40px;
    }
    .btn_wrap{
        margin-top: 40px;
        text-align: center;
    }
    .el-dropdown-menu__item p {
        text-align: center;
    }
</style>