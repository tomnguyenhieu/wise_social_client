<template>
    <div class="col-lg-6 col-md-8 no-pd">
        <div class="main-ws-sec">
            <div class="post-topbar">
                <textarea v-model="posts" name="post_area" id="post_area" class="form-control">何を考えていますか?</textarea>
                <div class="chose-image">
                    <ul>
                        <li>
                            <label for="btn_chose_file" class="btn btn-danger" title="Chose file .png or .jpeg">画像を選択</label>
                            <input @change="chooseImage" type="file" accept=".jpg, .jpeg" class="btn btn-danger" name="chose-file" id="btn_chose_file">
                        </li>
                    </ul>
                </div>
                <div class="post-st">
                    <ul>
                        <li><a class="active" v-on:click="submit()" href="#" title="Submit post data">送信</a></li>
                    </ul>
                </div>
                <!--post-st end-->
            </div>
            <!--post-topbar end-->
            <div class="posts-section">
                <div v-for="post in timelinePost" class="posty">
                    <div class="post-bar no-margin">
                        <div class="post_topbar">
                            <div class="usy-dt">
                                <img width="52px" v-if="post.author._avatar != null" :src="post.author._avatar" :alt="post.author.name"
                                    :title="post.author.name" />
                                <img width="52px" v-else src="../assets/images/resources/us-pc2.png" alt="">

                                <div class="usy-name">
                                    <h3>{{ post.author.name }}</h3>
                                    <span><img src="../assets/images/clock.png" alt="" />{{ post.since_created }}</span>
                                </div>
                            </div>
                            <div class="ed-opts">
                                <!-- Not my own post here none -->
                            </div>
                        </div>
                        <div class="epi-sec">
                            <ul class="descp">
                                <li><img src="../assets/images/icon8.png" alt=""><span>宇宙予言</span></li>
                                <li><img src="../assets/images/icon9.png" alt="">
                                    <span v-if="post.location">{{ post.location }}</span>
                                    <span v-else>Vietnam</span>
                                </li>
                            </ul>
                            <ul class="bk-links">
                                <li>
                                    <p v-if="post.favorites.length > 0" href="#" title="" v-on:click="removeFavorite(post.id)">
                                    <i class="fa fa-bookmark"></i>
                                    </p>
                                    <p v-else href="#" title="" v-on:click="favorite(post.id)">
                                    <i style="background-color: grey;" class="fa fa-bookmark"></i>
                                    </p>
                                </li>
                            </ul>
                        </div>
                        <div class="job_descp">
                            <div class="content" v-if="post.short_content && showReadMore.indexOf(post.id) === -1">
                                {{ post.short_content }}
                                <span class="text-danger txt-seemore" v-on:click="readMore(post.id)">。。。</span>
                            </div>
                            <div class="content" v-else>{{ post.content }}</div>
                            <br>
                            <ul class="skill-tags" v-if="post.skills.length > 0">
                                <li v-for="skill in post.skills">
                                    <a href="#" title="">{{ skill.skill }}</a>
                                </li>
                            </ul>
                        </div>
                        <div class="job-status-bar">
                            <ul class="like-com">
                                <li>
                                    <a class="color-b2b2b2 cusror-poiter hover-color"><i class="fa fa-heart"></i> いいね ({{ post.total_like }})</a>
                                </li>
                                <li>
                                    <a class="color-b2b2b2 cusror-poiter hover-color"><i class="fa fa-comment-alt"></i> コメント ({{post.total_comment}})</a>
                                </li>
                            </ul>
                            <a href="#"><i class="fa fa-eye"></i>閲覧数 ({{ post.view_count }})</a>
                        </div>
                    </div>
                    <!--post-bar end-->
                    <div class="comment-section">
                        <div class="comment-sec">
                            <ul>
                                <li>
                                    <div class="comment-list">
                                        <div class="bg-img">
                                            <img class="ava-cmt" src="../assets/images/resources/us-pc2.png" alt="">
                                        </div>
                                        <div class="comment">
                                            <h3>Minh Chính Phạm</h3>
                                            <span><img src="../assets/images/clock.png" alt=""> 05月06日</span>
                                            <p>中際提けトイつ売</p>
                                            <a href="#" title="" class="active"><i class="fa fa-reply-all"></i>返信</a>
                                        </div>
                                    </div>
                                    <!--comment-list end-->
                                    <!-- Reply begin -->
                                    <ul>
                                        <li>
                                            <div class="comment-list">
                                                <div class="bg-img">
                                                    <img src="../assets/images/resources/bg-img2.png" alt="">
                                                </div>
                                                <div class="comment">
                                                    <h3>Phúc Nguyễn Xuân</h3>
                                                    <span><img src="../assets/images/clock.png" alt=""> 05月06日</span>
                                                    <p>中際提けトイつ売主ケキ膀認ねず務法債ぞろ惜禁け割門9要 </p>
                                                    <a href="#" title=""><i class="fa fa-reply-all"></i>返信</a>
                                                </div>
                                            </div>
                                            <!--comment-list end-->
                                        </li>
                                    </ul>
                                    <!-- End reply -->
                                </li>
                                <li>
                                    <div class="comment-list">
                                        <div class="bg-img">
                                            <img src="../assets/images/resources/bg-img3.png" alt="">
                                        </div>
                                        <div class="comment">
                                            <h3>Trần Dần</h3>
                                            <span><img src="../assets/images/clock.png" alt=""> 05月06日</span>
                                            <p>中際提けトイつ売</p>
                                            <p>ず務法債中際提けトイつ売ぞろ惜禁け割門9要ワ <i class="fa fa-heart"></i> </p>
                                            <a href="#" title=""><i class="fa fa-reply-all"></i>返信</a>
                                        </div>
                                    </div>
                                    <!--comment-list end-->
                                </li>
                            </ul>
                        </div>
                        <!--comment-sec end-->
                        <div class="post-comment">
                            <div class="comment_box">
                                <form>
                                    <input type="text" placeholder="コメント #タグ を入力">
                                    <button type="submit"><i class="fa fa-paper-plane"></i></button>
                                </form>
                            </div>
                        </div>
                        <!--post-comment end-->
                    </div>
                    <!--comment-section end-->
                </div>
                <!--posty end-->
                <div class="process-comm">
                    <div class="spinner">
                        <div class="bounce1"></div>
                        <div class="bounce2"></div>
                        <div class="bounce3"></div>
                    </div>
                </div>
                <!--process-comm end-->
            </div>
            <!--posts-section end-->
        </div>
        <!--main-ws-sec end-->
    </div>
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
                posts: "",
                postImage: null,
                token: sessionStorage.getItem("token"),
                offset: 0,
                limit: 6,
                timelinePost: [],
                showReadMore: []
            }
        },
        created() {
            /***********************************************************************************************************
             *********************** Initialize data when this component is used. **************************************
             **********************************************************************************************************/
            console.log('Init created component and call to function get data from api server.');
            this.timeLine();
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

            readMore(postId) {
                this.showReadMore.push(postId);
            },

            chooseImage (event) {
                let file = event.target.files[0];
                console.log(file);
                if (file.type != 'image/jpeg' && file.type != 'image/jpg') {
                    alert("ファイル形式は「png」または「jpg」を選択してください。");
                    return null;
                }
                if (file.size > 2048000) {
                    alert("最大ファイル「2mb」");
                }
                this.postImage = file;
            },
            
            async submit () {
                // if (this.posts == '' || this.posts == null) {
                //     alert("内容を入力してください。");
                //     return null;
                // }
                let formData = new FormData();
                formData.append('content', this.posts);
                formData.append('image', this.postImage);
                // Send to server
                try {
                    const callAPI = await axios.post('http://localhost/wise_social_api/public/api/create-post',
                    formData, {
                        /************ Attach param for request here ***************/
                        headers: {
                            "Content-type" : "application/form-data",
                            "Authorization": "Bearer " + this.token 
                        }
                    });
                    if (callAPI.data.code == 200) {
                        console.log(callAPI.data.code);
                        return null;
                    } else {
                        alert("Call api failed, please check again!");
                    }
                } catch (err) {
                    console.log(err);
                }

                console.log(this.posts);
                console.log(this.postImage);
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
            async callAPI() {
                try {
                    const callAPI = await axios.get('http://localhost/DemoAPI/public/api/list-user', {
                        /************ Attach param for request here ***************/
                        headers: {
                            "Content-type" : "application/json",
                            "Authorization": "Bearer 28|TK2GDxdOitONowsftHVRRJhZeYFBZR2tQwdJjoSKfe2d9037" 
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
                            "Content-type" : "application/json",
                            "Authorization": "Bearer 28|TK2GDxdOitONowsftHVRRJhZeYFBZR2tQwdJjoSKfe2d9037" 
                        }
                    });
                    this.callAPI();
                } catch (err) {
                    console.log(err);
                }
            },
            /**
             * 
             */
            async timeLine() {
                try {
                    const callAPI = await axios.get('http://localhost/wise_social_api/public/api/timeline', {
                        /************ Attach param for request here ***************/
                        headers: {
                            "Content-type" : "application/json",
                            "Authorization": "Bearer " + this.token 
                        },
                        params: {
                            offset: this.offset,
                            limit: this.limit
                        }
                    });
                    if (callAPI.data.code == 200) {
                        console.log(callAPI.data.code);
                        this.timelinePost = callAPI.data.data;
                    } else {
                        alert("Call api failed, please check again!");
                    }
                } catch (err) {
                    console.log(err);
                }
            },

            async favorite(postId) {
                try {
                    const callAPI = await axios.get('http://localhost/wise_social_api/public/api/add-favorites', {
                        /************ Attach param for request here ***************/
                        headers: {
                            "Content-type" : "application/json",
                            "Authorization": "Bearer " + this.token 
                        },
                        params: {
                            post_id:postId
                        }
                    });
                    if (callAPI.data.code == 200) {
                        const index = this.timelinePost.findIndex(post => post.id === postId);
                    if (index !== -1) {
                        this.timelinePost[index].favorites.push({ post_id: postId });
                    }
                    } else {
                        alert("Call api failed, please check again!");
                    }
                } catch (err) {
                    console.log(err);
                }
            },

            async removeFavorite(postId) {
                try {
                    const callAPI = await axios.get('http://localhost/wise_social_api/public/api/remove-favorites', {
                        /************ Attach param for request here ***************/
                        headers: {
                            "Content-type" : "application/json",
                            "Authorization": "Bearer " + this.token 
                        },
                        params: {
                            post_id:postId
                        }
                    });
                    if (callAPI.data.code == 200) {
                        const index = this.timelinePost.findIndex(post => post.id === postId);
                    if (index !== -1) {
                        this.timelinePost[index].favorites = [];
                    }
                    } else {
                        alert("Call api failed, please check again!");
                    }
                } catch (err) {
                    console.log(err);
                }
            },
        },
    }
</script>

<style>
/**
* Custom local style css
*/
.txt-readmore:hover {
    cursor: pointer;
}
</style>