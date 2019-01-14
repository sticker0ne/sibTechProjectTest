<template>
    <div class="post-view-modal">
        <div class="modal-backdrop fade show"></div>
        <div class="modal fade show">
            <div class="modal-dialog" role="document">
                <div class="modal-content">
                    <div class="modal-header">
                        <h4 class="d-inline-block header-title">Please log in</h4>
                    </div>
                    <div class="modal-body">
                        <div class="login-form pt-2">
                            <div v-if="errorMessage" class="text-danger mb-2">{{errorMessage}}</div>
                            <form v-on:submit.prevent>
                                <div class="form-group">
                                    <input type="text" class="form-control" placeholder="login" v-model="loginValue"
                                           required>
                                </div>
                                <div class="form-group">
                                    <input type="password" class="form-control" placeholder="password"
                                           v-model="passwordValue" required>
                                </div>
                                <button type="submit" class="btn btn-outline-primary login-btn" @click="login">Log in
                                </button>
                            </form>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
    export default {
        name: 'LoginModal',
        props: {
            errorMessage: null
        },
        data: function () {
            return {
                loginValue: null,
                passwordValue: null
            }
        },
        methods: {
            login: function () {
                this.$emit('login', this.loginValue, this.passwordValue)
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
        width: 100%;
    }

    .modal-dialog {
        width: 300px;
        margin: auto;
        padding-top: 100px;
    }

    .modal-header {
        padding-bottom: 13px;
        display: block;
    }

    .header-title {
        margin-left: 3%;
    }

    .modal-body {
        padding: 0;
    }

    .login-form {
        width: 90%;
        margin-left: 5%;
    }

    .login-btn {
        margin-bottom: 10px;
        width: 100%;
    }
</style>
