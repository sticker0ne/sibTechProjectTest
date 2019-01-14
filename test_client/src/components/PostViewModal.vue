<template>
    <div class="post-view-modal">
        <div class="modal-backdrop fade show" @click="$emit('toggle-post-view-modal')"></div>
        <div class="modal fade show">
            <div class="modal-dialog" role="document">
                <div class="modal-content">
                    <div class="modal-header">
                        <h2 class=" d-inline-block post-title">{{post.title}}</h2>
                        <button type="button"
                                class="btn btn-outline-secondary close-btn"
                                @click="$emit('toggle-post-view-modal')">Close
                        </button>
                    </div>
                    <div class="modal-body">
                        <img src="https://picsum.photos/900/300/?random&id" class="post-image">
                        <hr class="mt-0" width="100%">
                        <div class="post-full-text">
                            <p>{{post.text}}</p>
                            <button type="button"
                                    class="btn btn-outline-danger"
                                    @click="$emit('remove-post', post.id)">Delete post
                            </button>
                        </div>
                        <hr width="100%">
                        <div class="comment-block" v-if="comments.length > 0">
                            <p class="mt-2">{{'Comments ' + comments.length + ':'}}</p>
                            <Comment v-for="comment in comments"
                                     @remove-comment="removeComment"
                                     :comment="comment"></Comment>
                        </div>
                        <div class="comment-form-container">
                            <CommentForm @post-new-comment="postNewComment"></CommentForm>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
    import Comment from "@/components/Comment";
    import CommentForm from "@/components/CommentForm";
    import app from "@/App.vue"
    import axios from 'axios'

    export default {
        name: 'PostViewModal',
        components: {CommentForm, Comment},
        props: {
            post: Object,
            token: String
        },

        data: function () {
            return {
                comments: []
            }
        },

        mounted() {
            this.loadComments();
        },

        methods: {
            loadComments: function () {
                this.$emit('toggle-loading', true);
                axios
                    .get(app.data().corsProxyUrl + app.data().apiUrl + 'comments/' + this.post.id, {
                        params: {
                            post_id: this.post_id,
                        },
                        headers: {
                            'x-request-token': this.token,
                        }
                    })
                    .then(response => {
                        this.comments = response.data;
                        this.$emit('toggle-loading', false);
                    })
            },

            postNewComment: function (comment) {
                this.$emit('toggle-loading', true);
                axios
                    .post(app.data().corsProxyUrl + app.data().apiUrl + 'comments/' + this.post.id, {
                            payload: {
                                author: comment.author,
                                comment: comment.text
                            }
                        },
                        {
                            headers: {
                                'x-request-token': this.token
                            }
                        })
                    .then(() => {
                        comment.author = '';
                        comment.text = '';
                        this.loadComments();
                        this.$emit('toggle-loading', false);
                    })
            },

            removeComment: function (comment_id) {
                this.$emit('toggle-loading', true);
                axios
                    .delete(app.data().corsProxyUrl + app.data().apiUrl + 'comments/' + comment_id, {
                        headers: {
                            'x-request-token': this.token,
                        }
                    });

                this.comments = this.comments.filter((item) => {
                    return item.id !== comment_id
                });
                this.$emit('toggle-loading', false);
            }
        }
    }
</script>

<style scoped>

    .modal::-webkit-scrollbar {
        width: 0;
    }

    .modal {
        display: block;
        overflow: auto;
        width: 90%;
        margin-left: 5%;
    }

    .modal-dialog {
        max-width: none;
    }

    .modal-header {
        padding-bottom: 13px;
    }

    .modal-body {
        padding: 0;
    }

    .post-title {
        padding-top: 10px;
        margin-bottom: 0;
    }

    .post-image {
        width: 100%;
    }

    .post-full-text {
        width: 90%;
        margin-left: 5%;
    }

    .post-full-text p {
        white-space: pre-wrap;
    }

    .comment-block {
        width: 94%;
        margin-left: 3%;
    }

    .comment-form-container {
        width: 50%;
        margin-left: 3%;
        margin-top: 20px;
    }

    .close-btn {
        margin-top: 6px;
    }
</style>
