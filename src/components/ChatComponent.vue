<template>
	<!--chatbox-list end-->
	<div class="" id="section-chat">
		<div class="col-md-12 row chat-head"></div>
		<div class="col-md-12 chat-content-top">
			<h1 class="font-bold">メッセージ</h1>
		</div>
		<hr>
		<!-- List friends and message -->
		<div class="col-md-12 msgs-list">
			<!--msg-title end-->
			<div class="messages-list">
				<ul>
					<li v-for="friend in friends" v-bind:key="friend.id" v-on:click="showChatBox(friend)">
						<div class="usr-msg-details">
							<div class="usr-ms-img">
								<img v-if="friend.avatar != null" :src="friend.avatar" alt="">
								<img v-else src="../assets/images/resources/m-img1.png" alt="">
							</div>
							<div class="usr-mg-info">
								<h3>{{ friend.name }}</h3>
								<p>{{ friend.last_message }}<img src="../assets/images/smley.png" alt=""></p>
							</div>
							<!--usr-mg-info end-->
							<span class="posted_time">{{ friend.last_sent }}</span>
						</div>
						<!--usr-msg-details end-->
					</li>
				</ul>
			</div>
			<!--messages-list end-->
		</div>
		<div class="clear-fix"></div>
		<div class="col-md-12 chat-content-foobar">
			<img class="fl-rgt" src="../assets/images/logo2.png" alt="">
		</div>
	</div>
	<div v-if="isShowChatBox" class="chat-box">
		<div class="chat-box-head">
			<div class="chat-box-head-friend">
				<img class="chat-box-friend-avatar" src="../assets/images/resources/s1.png" alt="">
				<p>{{ selectedFriend.name != null ? selectedFriend.name : " " }}</p>
			</div>
			<div class="chat-box-close" v-on:click="closeChatBox()">
				<p class="fa fa-times"></p>
			</div>
		</div>
		<div class="chat-box-content">
			<div v-for="msg in listMessage" v-bind:key="msg.id">
				<div class="my-msg" v-if="msg.my_message == 'me'">
					<p>{{ msg.message }}</p>
					<span class="send-date">{{ msg._created_at }}</span>
				</div>
				<div class="friend-msg" v-if="msg.my_message == 'friend'">
					<p>{{ msg.message }}</p>
					<span class="send-date">{{ msg._created_at }}</span>
				</div>
			</div>
		</div>
		<div class="chat-box-send-msg">
			<div class="chat-box-text-field">
				<input v-model="messageContent" type="text" class="form-control cls-text-field" placeholder="Type...">
			</div>
			<div class="chat-box-send-btn" v-on:click="sendMessage()">
				<i class="fa fa-send"></i>
			</div>
		</div>
	</div>
	<div class="section-ads">
		<img src="https://media2.giphy.com/media/GXwvDVrQTCKAUBu71D/giphy.gif?cid=790b761152aa12f58ec81be0f7a66f9edeadf7ede51e7dd1&rid=giphy.gif&ct=g"
			alt="">
	</div>
</template>

<script>
	import axios from 'axios';

	export default {
		sockets: {
			ClientSendMessageToServer: function (responseMessage) {
				this.messageContent = responseMessage;
			},
			ServerSendMessageToClient: function (responseMessage) {
				if (responseMessage.type === 'message') {
					if (responseMessage.user_id == this.selectedFriend.id) {
						responseMessage.my_message = "friend";
					}
					this.listMessage.push(responseMessage);
					this.$forceUpdate();
				}
			},
		},
		data() {
			return {
				token: sessionStorage.getItem("token"),
				friends: [],
				isShowChatBox: false,
				selectedFriend: null,
				listMessage: [],
				roomId: 0,
				messageContent: ""
			}
		},
		created() {
			this.getFriends()
		},
		watch: {
			selectedFriend() {
				this.getMessages();
			}
		},
		methods: {
			async getFriends() {
				try {
					const callAPI = await axios.get('http://localhost/wise_social_api/public/api/list-friend', {
						/************ Attach param for request here ***************/
						headers: {
							"Content-type": "application/json",
							"Authorization": "Bearer " + this.token
						}
					});
					if (callAPI.data.code == 200) {
						this.friends = callAPI.data.data;
					} else {
						alert("Call api failed, please check again!");
					}
				} catch (err) {
					console.log(err);
				}
			},
			showChatBox(friend) {
				this.isShowChatBox = true;
				this.selectedFriend = friend;
			},
			closeChatBox() {
				this.isShowChatBox = false;
				this.selectedFriend = null;
			},
			async getMessages() {
				try {
					const callAPI = await axios.get('http://localhost/wise_social_api/public/api/list-message', {
						/************ Attach param for request here ***************/
						params: {
							'friend_id': this.selectedFriend.id
						},
						headers: {
							"Content-type": "application/json",
							"Authorization": "Bearer " + this.token
						}
					});
					if (callAPI.data.code == 200) {
						this.listMessage = callAPI.data.data.messages;
						this.roomId = callAPI.data.data.room_id;
						let dataMsgSentToSocket = {
							"id": 0,
							"name": "",
							"avatar": "",
							"message": "",
							"user_id": "",
							"friend_id": "",
							"is_view": 0,
							"created_at": "",
							"my_message": "me",
							"_created_at": "",
							"room_id": this.roomId,
							"type": "message",
							"action": "join"
						}
						this.$socket.emit(
							'ClientSendMessageToServer',
							dataMsgSentToSocket
						);
					} else {
						alert("Call api failed, please check again!");
					}
				} catch (err) {
					console.log(err);
				}
			},
			async sendMessage() {
				try {
					let formData = new FormData();
					formData.append('friend_id', this.selectedFriend.id);
					formData.append('room_id', this.roomId);
					formData.append('message_content', this.messageContent);
					const callAPI = await axios.post('http://localhost/wise_social_api/public/api/send-message',
						formData, {
						/************ Attach param for request here ***************/
						headers: {
							"Content-type": "multipart/form-data",
							"Authorization": "Bearer " + this.token
						}
					});
					if (callAPI.data.code == 200) {
						this.messageContent = "";
						this.$socket.emit(
							'ClientSendMessageToServer',
							callAPI.data.data
						);
					} else {
						alert("Call api failed, please check again!");
					}
				} catch (err) {
					console.log(err);
				}
			}
		}
	}
</script>