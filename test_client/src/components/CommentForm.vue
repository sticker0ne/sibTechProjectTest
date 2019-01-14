<template>
    <div class="comment-form pb-3">
        <h4>Post a comment</h4>
        <form v-on:submit.prevent="$emit('post-new-comment', comment)">
            <div class="form-group">
                <label for="inputName">Name</label>
                <input v-model="comment.author" type="text" class="form-control" id="inputName" placeholder="Name"
                       required>
            </div>
            <div class="form-group">
                <label for="comment">Comment</label>
                <textarea v-model="comment.text" class="form-control" id="comment" placeholder="Comment"
                          required></textarea>
            </div>
            <button type="submit" class="btn btn-outline-primary">Post comment</button>
        </form>
    </div>
</template>

<script>
    export default {
        name: "CommentForm",
        data: function () {
            return {
                comment: {
                    author: '',
                    text: ''
                }
            }
        }
    }
</script>

<style scoped>
    #inputName {
        width: 50%;
    }
</style>