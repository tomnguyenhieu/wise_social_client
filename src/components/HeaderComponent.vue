<template>
	<header>
		<div class="container">
			<div class="header-data">
				<div class="logo">
					<RouterLink to="/auth">
						<img src="../assets/images/logo.png" alt="">
					</RouterLink>
				</div>
				<!--logo end-->
				<div class="search-bar">
					<form>
						<input v-on:keyup="search()" type="text" name="search" placeholder="検索..." v-model="keyWord">
					</form>
					<div v-if="searchStatus" class="" id="area-chat">
						<div v-for="user in users" class="account-item">
							<div class="avatar">
								<img v-if="user.avatar == null" src="../assets/images/resources/user2.png" alt="">
								<img v-else :src="user.avatar" alt="">
							</div>
							<div class="infor">
								<div class="name">{{ user.name }}</div>
								<div class="exp">{{ user.experience }}</div>
							</div>
						</div>
					</div>
				</div>
				<!--search-bar end-->
				<nav>
					<ul>
						<li>
							<a href="messages.html" title="" class="not-box-openm">
								<span><img src="../assets/images/icon6.png" alt=""></span>
								メッセージ
							</a>
						</li>
						<li>
							<a href="#" title="" class="not-box-open">
								<span><img src="../assets/images/icon7.png" alt=""></span>
								通知
							</a>
							<div class="notification-box noti" id="notification">
								<div class="nt-title">
									<a href="#" title="">クリア</a>
								</div>
								<div class="nott-list">
									<div class="notfication-details">
										<div class="noty-user-img">
											<img src="../assets/images/resources/ny-img1.png" alt="">
										</div>
										<div class="notification-info">
											<h3><a href="#" title="">Jassica William</a> プロジェクトにコメントします。</h3>
											<span>05月07日</span>
										</div>
										<!--notification-info -->
									</div>
									<div class="notfication-details">
										<div class="noty-user-img">
											<img src="../assets/images/resources/ny-img2.png" alt="">
										</div>
										<div class="notification-info">
											<h3><a href="#" title="">Jassica William</a> プロジェクトにコメントします。</h3>
											<span>05月07日</span>
										</div>
										<!--notification-info -->
									</div>
									<div class="notfication-details">
										<div class="noty-user-img">
											<img src="../assets/images/resources/ny-img3.png" alt="">
										</div>
										<div class="notification-info">
											<h3><a href="#" title="">Jassica William</a> プロジェクトにコメントします。</h3>
											<span>05月07日</span>
										</div>
										<!--notification-info -->
									</div>
									<div class="notfication-details">
										<div class="noty-user-img">
											<img src="../assets/images/resources/ny-img2.png" alt="">
										</div>
										<div class="notification-info">
											<h3><a href="#" title="">Jassica William</a> プロジェクトにコメントします。</h3>
											<span>05月07日</span>
										</div>
										<!--notification-info -->
									</div>
								</div>
								<!--nott-list end-->
							</div>
							<!--notification-box end-->
						</li>
					</ul>
				</nav>
				<!--nav end-->
				<div class="menu-btn">
					<a href="#" title=""><i class="fa fa-bars"></i></a>
				</div>
				<!--menu-btn end-->
				<div class="user-account">
					<div class="user-info">
						<img src="../assets/images/resources/user.png" alt="">
						<a href="index.html" title="">上田梓</a>
						<i class="la la-sort-down"></i>
					</div>
					<div class="user-account-settingss" id="users">
						<!--search_form end-->
						<h3 class="tc"><a href="my-profile-feed.html">一般設定</a></h3>
						<h3 class="tc"><a href="sign-in.html" title="">ログアウト</a></h3>
					</div>
					<!--user-account-settingss end-->
				</div>
			</div>
			<!--header-data end-->
		</div>
	</header>
	<!--header end-->
</template>

<script>
	//import Vue from 'vue'
	import axios from 'axios'
	// import component1 from 'component1'
	// import component2 from 'component2'

	export default {
		/***********************************************************************************************************
		 ******************************* Pass data to child component **********************************************
		 **********************************************************************************************************/
		// props: [variable1, variable2],
		// components: {component1, component2},
		data() {
			/***********************************************************************************************************
			 ******************************* Initialize global variables ***********************************************
			 **********************************************************************************************************/
			return {
				msg: 'Hello world!',
				users: [],
				token: sessionStorage.getItem("token"),
				keyWord: '',
				searchStatus: false
			}
		},
		created() {
			/***********************************************************************************************************
			 *********************** Initialize data when this component is used. **************************************
			 **********************************************************************************************************/

		},
		mounted() {
			/***********************************************************************************************************
			 ******************** Once created, the interface is displayed and calls mounted. **************************
			 **********************************************************************************************************/
		},
		watch: {
			/***********************************************************************************************************
			 ********************************* Methods change value for a variable *************************************
			 **********************************************************************************************************/
			msg() {
				console.log("When the value of the msg variable changes, this method will be executed.");
			}
		},
		computed: {
			appendMsg() {
				return msg + "Process the value and assign the value to the corresponding variable the var has changed.";
			}
		},
		methods: {
			/***********************************************************************************************************
			 ******************************* Default functions that handle local data **********************************
			 **********************************************************************************************************/

			/**
			 * Example default function not using param
			 */
			defaultFunction() {
				this.msg = "Replace message here!";
			},
			/**
			 * Example default function using param
			 *
			 * @param int pageNum number of page
			 * @return boolean
			 */
			defaultFunctionUsingParam(pageNum) {
				console.log(pageNum);
				return false;
			},

			/***********************************************************************************************************
			 ******* Async and await functions for manipulating server-side data through internal API protocols ********
			 **********************************************************************************************************/

			/**
			 * Call API sample
			 */
			async search() {
				this.searchStatus = true;
				try {
					const callAPI = await axios.get('http://localhost/wise_social_api/public/api/search', {
						/************ Attach param for request here ***************/
						headers: {
							"Content-type": "application/json",
							"Authorization": "Bearer " + this.token
						},
						params: {
							"key-word": this.keyWord
						}
					});
					if (callAPI.data.code == 200) {
						this.users = callAPI.data.data;
					} else {
						alert("Call api failed, please check again!");
					}
				} catch (err) {
					console.log(err);
				}
			},
			async deleteUser(id) {
				try {
					const callAPI = await axios.delete('http://localhost/DemoAPI/public/api/delete-user/' + id, {
						/************ Attach param for request here ***************/
						headers: {
							"Content-type": "application/json",
							"Authorization": "Bearer 28|TK2GDxdOitONowsftHVRRJhZeYFBZR2tQwdJjoSKfe2d9037"
						}
					});
					this.callAPI();
				} catch (err) {
					console.log(err);
				}
			}
		},
	}
</script>

<style>
	#area-chat {
		height: 530px;
		background: ghostwhite;
		border: gray;
		width: 500px;
		margin-top: 40px;
		position: absolute;
		z-index: 9;
		overflow: auto;
	}

	.account-item {
		border-bottom: 1px dotted gray;
		display: flex;
		padding-bottom: 15px;
	}

	.account-item .avatar {
		width: 15%;
	}

	.account-item img {
		margin-top: 15px;
		margin-left: 15px;
		width: 56px;
	}

	.account-item .infor {
		padding-left: 15px;
		width: 85%;
	}

	.account-item .infor .name {
		font-size: 22px;
		font-weight: bold;
		margin-top: 15px;
	}

	.account-item .infor .exp {
		font-size: 14px;
		font-style: italic;
		margin-top: 5px;
	}

	.account-item:hover {
		background: white;
		cursor: pointer;
	}
</style>