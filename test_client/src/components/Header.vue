<template>
    <div class="header">
        <hr width="95%">
        <div class="col-10 offset-1">
            <div class="d-inline-block">
                <h2>{{text}}</h2>
            </div>
            <div class="d-inline-block buttons">
                <button class="btn btn-outline-success mt-3 mb-3 mr-2"
                @click="$emit('load-more-posts')">Load 5 more</button>
                <button class="btn btn-outline-primary mt-3 mb-3"
                        @click="$emit('toggle-post-write-modal')">
                    Write new post
                </button>
                <button class="btn btn-outline-danger mt-3 mb-3 ml-5"
                        @click="$emit('log-out')">
                    Log out
                </button>
            </div>
        </div>
        <hr class="lower-line" width="95%">
    </div>
</template>

<script>
    export default {
        name: 'Header',
        props: {
            text: String
        }
    }
</script>

<style scoped>
    .buttons {
        float: right;
    }

    .btn {
        margin-top: 0 !important;
        margin-bottom: 0 !important;
    }

    .lower-line {
        margin-top: 10px !important;
    }
</style>
