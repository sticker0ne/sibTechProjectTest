<template>
    <div class="pb-5">
        <div class="row">
            <div class="col-8 offset-2">
                <div class="post border">
                    <img :src="'https://picsum.photos/900/300/?random&id='+ post.id" class="post-image">
                    <div class="post-meta pl-3 pr-3">
                        <div class="d-inline-block post-title">
                            <p class="h4">{{post.title}}</p>
                        </div>
                        <div class="d-inline-block float-right post-created-at">
                            <p class="text-secondary font-weight-light">{{formatDate(post.created_at)}}</p>
                        </div>
                        <hr align="center" width="100%"/>
                        <div class="post-short-text">
                            <p class="font-weight-light">{{post.short_text}}</p>
                        </div>
                        <button class="btn btn-outline-primary mt-3 mb-3"
                                @click="$emit('toggle-post-view-modal', post)">Read →
                        </button>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
    import moment from 'moment'

    export default {
        name: 'PostPreview',
        props: {
            post: Object
        },
        methods: {
            formatDate(date) {
                return (moment(date, 'DD.MM.YYYY HH:mm').format('DD MMM YYYY HH:mm'));
            }
        }
    }
</script>

<style scoped>
    .post {
        border-radius: 10px;
    }

    .post-image {
        width: 100%;
        border-top-left-radius: 10px;
        border-top-right-radius: 10px;
        padding-bottom: 10px;
    }

    p {
        margin-bottom: 0 !important;
    }

    hr {
        margin-top: 10px !important;
        margin-bottom: 10px !important;
    }

    .row {
        margin-left: 0;
        margin-right: 0;
    }
</style>
