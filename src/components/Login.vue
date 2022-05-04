<template>
  <div>
    登陆
  </div>
</template>
<script>
    export default{
        data () {
            return {
                rules: {
                    username: [{required: true, message: '用户名不能为空', trigger: 'blur'}],
                    password: [{required: true, message: '密码不能为空', trigger: 'blur'}]
                },
                checked: true,
                loginForm: {
                    username: '',
                    password: ''
                },
                loading: false
            }
        },
        methods: {
            login () {
                var _this = this
                this.$axios
                    .post('/login', {
                        username: this.loginForm.username,
                        password: this.loginForm.password
                    })
                    .then(resp => {
                        if (resp.data.code === 200) {
                            var data = resp.data.result
                            _this.$store.commit('login', data)
                            var path = _this.$route.query.redirect
                            sessionStorage.setItem("DIS_username",this.loginForm.username)
                            _this.$router.replace({path: path === '/' || path === undefined ? '/admin/dashboard' : path})
                        } else {
                            this.$alert(resp.data.message, '提示', {
                                confirmButtonText: '确定'
                            })
                        }
                    })
                    .catch(failResponse => {
                        failResponse.errors
                    })
            }
        }
    }
</script>
<style>
    #paper {
        background: url("../assets/img/bg/bg.jpg") no-repeat center;
        height: 100%;
        width: 100%;
        background-size: cover;
        position: fixed;
    }
    body{
        margin: 0;
    }
    .login-container {
        border-radius: 15px;
        background-clip: padding-box;
        margin: 90px auto;
        width: 350px;
        padding: 35px 35px 15px 35px;
        background: #fff;
        border: 1px solid #eaeaea;
        box-shadow: 0 0 25px #cac6c6;
    }
    .login_title {
        margin: 0px auto 40px auto;
        text-align: center;
        color: #505458;
    }
    .login_remember {
        margin: 0px 0px 35px 0px;
        text-align: left;
    }
</style>