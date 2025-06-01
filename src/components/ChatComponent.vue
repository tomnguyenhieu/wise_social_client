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
					<li v-for="friend in friends" v-bind:key="friend.id">
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
	<div class="chat-box">
		<div class="chat-box-head">
			<div class="chat-box-head-friend">
				<img class="chat-box-friend-avatar" src="../assets/images/resources/s1.png" alt="">
				<p>香野綾花</p>
			</div>
			<div class="chat-box-close">
				<p class="fa fa-times"></p>
			</div>
		</div>
		<div class="chat-box-content">
			<div class="my-msg">
				<p>
					主ケキ膀認ねず務法債中際提けトイつ売ぞろ惜禁け割門9要ワキ家際ほもば木久まき容純菜ぎぽぞ載動宅略石れざきこ。
				</p>
				<span class="send-date">2024-12-26 11:56</span>
			</div>
			<div class="friend-msg">
				<p>
					キ家際ほもば木久まき容純菜ぎぽぞ。
				</p>
				<span class="send-date">2024-12-26 11:56</span>
			</div>
			<div class="my-msg">
				<p>
					中際提けトイつ売主ケキ膀認ねず務法債ぞろ惜禁け割門9要ワキ家際ほもば木久まき容純菜ぎぽぞ載動宅略石れざきこ。
				</p>
				<span class="send-date">2024-12-26 11:56</span>
			</div>
			<div class="my-msg">
				<p>
					宅略石れざきこ。合そいやぶ間準ヌイヘミ罪高そ県1人スヲナ月7神いこ港彬ホヨアタ最非3争ミイホヘ生要きゅ熱覧ょ式司フホメ究真メスチコ革面じ訓準晶ぼフを。
				</p>
				<span class="send-date">2024-12-26 11:56</span>
			</div>
			<div class="friend-msg">
				<p>
					月7神いこ港彬ホヨアタ最非3争ミイホヘ生要きゅ熱覧ょ式司フホメ。
				</p>
				<span class="send-date">2024-12-26 11:56</span>
			</div>
		</div>
		<div class="chat-box-send-msg">
			<div class="chat-box-text-field">
				<input type="text" class="form-control cls-text-field" placeholder="">
			</div>
			<div class="chat-box-send-btn">
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
		data() {
			return {
				token: sessionStorage.getItem("token"),
				friends: []
			}
		},
		created() {
			this.getFriends()
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
		}
	}
</script>