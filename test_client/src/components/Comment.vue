<template>
    <div class="comment">
        <p class="text-secondary d-inline-block comment-author">{{comment.author}}</p>
        <p class="text-secondary d-inline-block comment-created">{{formatDate(comment.created_at)}}</p>
        <p class="font-weight-light mb-0 comment-text">{{comment.comment}}</p>
        <div class="buttons">
            <button class="btn btn-outline-danger delete-btn"
                    @click="$emit('remove-comment', comment.id)">Delete
            </button>
        </div>
        <hr width="100%">
    </div>
</template>

<script>
    import moment from 'moment'

    export default {
        name: "Comment",
        props: {
            comment: Object
        },
        methods: {
            formatDate(date) {
                return (moment(date, 'DD.MM.YYYY HH:mm').format('DD MMM YYYY HH:mm'));
            }
        }
    }
</script>

<style scoped>
    .comment-created {
        float: right;
    }

    .comment-text {
        white-space: pre-wrap;
    }

    .buttons {
        text-align: right;
        margin-top: 5px;
    }
</style>