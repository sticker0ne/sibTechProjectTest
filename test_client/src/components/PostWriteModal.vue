<template>
    <div class="post-view-modal">
        <div class="modal-backdrop fade show" @click="$emit('toggle-post-write-modal')"></div>
        <div class="modal fade show">
            <div class="modal-dialog" role="document">
                <div class="modal-content">
                    <div class="modal-header">
                        <h4 class="d-inline-block header-title">New post</h4>
                        <button type="button"
                                class="btn btn-outline-secondary header-button"
                                data-dismiss="modal"
                                @click="$emit('toggle-post-write-modal')">Close
                        </button>
                    </div>
                    <div class="modal-body">
                        <div class="comment-form pt-2">
                            <form v-on:submit.prevent="$emit('create-new-post', post)">
                                <div class="form-group">
                                    <label for="title">Title</label>
                                    <input v-model="post.title" type="text" class="form-control" id="title"
                                           placeholder="Title" required>
                                </div>
                                <div class="form-group">
                                    <label for="content">Content</label>
                                    <textarea v-model="post.text" class="form-control" id="content"
                                              placeholder="Content" rows="5"
                                              required></textarea>
                                </div>
                                <button type="submit" class="btn btn-outline-primary">Post it</button>
                            </form>
                        </div>
                    </div>
                    <hr width="100%">
                </div>
            </div>
        </div>
    </div>
</template>

<script>
    export default {
        name: 'PostWriteModal',
        data: function () {
            return {
                post: {
                    title: '',
                    text: ''
                }
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
        width: 60%;
        margin-left: 20%;
        height: auto;
    }

    .modal-dialog {
        max-width: none;
    }

    .modal-header {
        padding-bottom: 13px;
        display: block;
    }

    .header-title {
        margin-left: 3%;
    }

    .header-button {
        float: right;
        margin-right: 1%;
    }

    .modal-body {
        padding: 0;
    }

    .comment-form {
        width: 90%;
        margin-left: 5%;
    }
</style>
