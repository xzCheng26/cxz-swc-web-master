<template>
  	<div class="login_page fillcontain">
		<div style="height:50px;" role="navigation">
		  <nav class="navbar navbar-static-top">
			  <div class="navbar-header">
				<img src="../assets/image/log2.png" class="img-responsive">
			</div>
		</nav>
	  </div>
		<transition name="el-fade-in">
	  		<section class="form_contianer" v-show="showLogin">
		    	<el-form :model="loginForm" :rules="rules" ref="loginForm" @keyup.enter.native="submitForm('loginForm')">
					<h2>用户登录</h2>
					<div class="row">
						<div class="col-sm-6">
							<div class="logimg">
								<img src="../assets/image/xsqq.png">
							</div>
						</div>
						<div class="col-sm-6">
							<el-form-item prop="username">
								<el-input v-model="loginForm.username" placeholder="用户名" suffix-icon="el-icon-user"></el-input>
							</el-form-item>
							<el-form-item prop="password">
								<el-input type="password" placeholder="密码" v-model="loginForm.password" suffix-icon="el-icon-lock"></el-input>
							</el-form-item>
							<el-form-item>
								<el-button type="primary" v-loading.fullscreen.lock="fullscreenLoading" @click="submitForm('loginForm')" class="submit_btn">登录</el-button>
							</el-form-item>
							<div>账号/密码： admin/123456</div>
						</div>
					</div>
				</el-form>
	  		</section>
		</transition>
  	</div>
</template>

<script>
	import {baseUrl} from '@/common/biConfig'
	import $ from 'jquery'

	export default {
	    data(){
			return {
				loginForm: {
					username: '',
					password: '',
				},
				fullscreenLoading: false,
				rules: {
					username: [
			            { required: true, message: '请你输入用户名', trigger: 'blur' },
			        ],
					password: [
						{ required: true, message: '请输入密码', trigger: 'blur' }
					],
				},
				showLogin: true
			}
		},
		mounted(){
		},
		computed: {
		},
		methods: {
			submitForm: function(formName) {
				let ts = this;
				this.$refs[formName].validate(async (valid) => {
					if (valid) {
						ts.fullscreenLoading = true;
						$.ajax({
							type:"POST",
							data:{userName:this.loginForm.username, password:this.loginForm.password},
							dataType:"JSON",
							url:baseUrl+"doLogin.action",
							xhrFields: {withCredentials: true},
    						crossDomain: true,
							success:function(resp){
								ts.fullscreenLoading = false;
								if (resp.result == 1) {
									//清除菜单缓存
									localStorage.removeItem("menus");
									ts.$router.push('Welcome')
								}else{
									ts.$notify.error({
										title: '登录错误',
										message: resp.msg,
										offset: 50
									});
								}
							},
							error:function(){
								ts.fullscreenLoading = false;
								ts.$notify.error({
										title: '网络错误',
										offset: 50
									});
							}
						});
					} else {
						return false;
					}
				});
			},
			fadeIn:function(){
				 this.showLogin = !this.showLogin;
			}
		},
		watch: {

		}
	}
</script>

<style lang="less" scoped>
	@import '../style/mixin';
	.login_page{
		background-color: #f0f3f4;
		background:url("../assets/image/login_bg.jpg") 100% center
	}
	.form_contianer{
		max-width: 600px;
		padding: 20px;
		top: 40%;
		margin: 0 auto;
    	position: relative;
    	transform: translateY(-55%);
 		box-shadow: 0 2px 5px 0 rgba(0, 0, 0, 0.16), 0 2px 10px 0 rgba(0, 0, 0, 0.12);
    	border-radius: .5rem;
		background-color: #fff;
		text-align: center;
		.submit_btn{
			width: 100%;
			font-size: 16px;
		}
		h2 {
			font-size: 24px;
			padding: 0 20px 20px 20px;
			font-weight: normal;
		}
	}
	.tip{
		font-size: 12px;
		color: red;
	}
	.logimg {
    	padding:20px 10px 20px 0px;
	}
	@media (max-width: 600px) {
		.form_contianer {
			max-width: 400px;
		}
		.logimg {
			padding:5px 0px 20px 0px;
		}
	}
	@media (max-width: 500px) {
		.form_contianer {
			max-width: 300px;
		}
		.logimg {
			padding:5px 0px 20px 0px;
		}
	}
	.navbar-static-top {
		background-color: black;
	}
	.bottomtxt{
		bottom:0px;
		width: 100%;
		position: absolute;
		.txt {
			color:#fff;
		}
		a {
			color:#fff;
		}
	}
</style>
