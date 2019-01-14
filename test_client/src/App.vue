<template>
    <div id="app">
        <Loader v-if="loading"></Loader>
        <LoginModal v-if="!userIsLoggedIn" @login="getToken"
                    :errorMessage="loginErrorMessage"
                    @toggle-loading="toggleLoading"></LoginModal>
        <div v-if="userIsLoggedIn">
            <Header text="Useless blog"
                    @toggle-post-write-modal="togglePostWriteModal"
                    @log-out="deAuthUser"
                    @toggle-loading="toggleLoading"
                    @load-more-posts="loadMorePosts"></Header>
            <PostPreview v-for="post in posts"
                         :post="post"
                         v-on:toggle-post-view-modal="togglePostViewModal"></PostPreview>
            <PostViewModal v-if="postViewModalVisibility"
                           @toggle-post-view-modal="togglePostViewModal"
                           @toggle-loading="toggleLoading"
                           @remove-post="removePost"
                           :post="selectedPost"
                           :token="token"></PostViewModal>
            <PostWriteModal v-if="postWriteModalVisibility"
                            @toggle-post-write-modal="togglePostWriteModal"
                            @create-new-post="createNewPost"
                            @toggle-loading="toggleLoading"></PostWriteModal>
        </div>
    </div>
</template>

<script>
    import PostPreview from './components/PostPreview.vue'
    import Header from "@/components/Header";
    import PostViewModal from "@/components/PostViewModal";
    import PostWriteModal from "@/components/PostWriteModal";
    import LoginModal from "@/components/LoginModal";
    import axios from 'axios'
    import Loader from "@/components/Loader";

    export default {
        name: 'app',
        components: {
            Loader,
            LoginModal,
            PostWriteModal,
            PostViewModal,
            Header,
            PostPreview
        },
        data: function () {
            return {
                corsProxyUrl: 'https://cors-anywhere.herokuapp.com/',
                apiUrl: 'http://stpb.exams.mover.run/',
                loginErrorMessage: null,
                postViewModalVisibility: false,
                postWriteModalVisibility: false,
                userIsLoggedIn: false,
                token: null,
                posts: [],
                selectedPost: null,
                loading: false
            }
        },
        mounted() {
            this.loadAndValidateToken();
        },
        methods: {
            togglePostViewModal: function (post) {
                this.selectedPost = post;
                this.postViewModalVisibility = !this.postViewModalVisibility;
            },

            togglePostWriteModal: function () {
                this.postWriteModalVisibility = !this.postWriteModalVisibility;
            },

            toggleLoading: function (status) {
                if (typeof status === 'boolean') {
                    this.loading = status;
                } else {
                    this.loading = !this.loading;
                }
            },

            getToken: function (login, password) {
                this.toggleLoading(true);
                let body = {
                    payload: {
                        login: login,
                        pass: password
                    }
                };
                axios
                    .post(this.corsProxyUrl + this.apiUrl + 'auth', body)
                    .then(response => {
                        this.storeToken(response.data.token);
                        this.toggleLoading(false);
                    })
                    .catch(() => {
                        this.loginErrorMessage = 'Login or password is incorrect';
                        this.toggleLoading(false);
                    });
            },

            storeToken: function (token) {
                console.log('storeToken');
                this.token = token;
                localStorage.token = token;
                this.loginErrorMessage = null;
                this.userIsLoggedIn = true;
                this.loadPosts(5, 0);
            },

            loadAndValidateToken: function () {
                console.log('loadAndValidate');
                if (localStorage.token) {
                    console.log('loadAndValidate token is true');
                    this.token = localStorage.token;
                    this.userIsLoggedIn = true;
                    this.loadPosts(5, 0);
                }
            },

            deAuthUser() {
                localStorage.removeItem('token');
                this.loginErrorMessage = null;
                this.userIsLoggedIn = false;
            },

            loadPosts(limit, offset) {
                this.toggleLoading(true);
                axios
                    .get(this.corsProxyUrl + this.apiUrl + 'posts', {
                        params: {
                            limit: limit,
                            offset: offset
                        },
                        headers: {
                            'x-request-token': this.token,
                        }
                    })
                    .then(response => {
                        this.posts = response.data;
                        this.toggleLoading(false);
                    })
            },

            createNewPost: function (post) {
                this.toggleLoading(true);
                axios
                    .post(this.corsProxyUrl + this.apiUrl + 'posts', {
                            payload: {
                                title: post.title,
                                text: post.text
                            }
                        },
                        {
                            headers: {
                                'x-request-token': this.token
                            }
                        })
                    .then(() => {
                        this.togglePostWriteModal();
                        this.toggleLoading(false);
                    })
                    .catch(error => console.log(error))
            },

            removePost: function (post_id) {
                this.toggleLoading(true);
                axios
                    .delete(this.corsProxyUrl + this.apiUrl + 'posts/' + post_id, {
                        headers: {
                            'x-request-token': this.token,
                        }
                    });

                this.posts = this.posts.filter((item) => {
                    return item.id !== post_id
                });

                this.togglePostViewModal();
                this.toggleLoading(false);
            },

            loadMorePosts: function () {
                this.toggleLoading(true);
                axios
                    .get(this.corsProxyUrl + this.apiUrl + 'posts', {
                        params: {
                            limit: 5,
                            offset: this.posts.length
                        },
                        headers: {
                            'x-request-token': this.token,
                        }
                    })
                    .then(response => {
                        this.posts = response.data.concat(this.posts);
                        this.toggleLoading(false);
                    })
            }
        }
    }
</script>

<style>
    #app {
    }
</style>
