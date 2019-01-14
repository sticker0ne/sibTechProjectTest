<template>
    <div class="loader-container">
        <three-dots class="loader"></three-dots>
        <div class="loader-backdrop"></div>
    </div>
</template>

<script>
    import ThreeDots from "vue-loading-spinner/src/components/ThreeDots";

    export default {
        name: "Loader",
        components: {ThreeDots}
    }
</script>

<style scoped>
    .loader-backdrop {
        position: fixed;
        top: 0;
        left: 0;
        z-index: 2004;
        width: 100vw;
        height: 100vh;
        background-color: black;
        opacity: .6;
    }

    .loader {
        position: fixed;
        top: calc(50% - 50px);
        left: calc(50% - 50px);
        width: 100px !important;
        height: 100px !important;
        z-index: 20010;
        opacity: 1;
    }
</style>