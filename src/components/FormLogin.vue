<template>

    <b-modal
            title="Форма авторизации"
            id="modal-center"
            centered no-close-on-backdrop no-close-on-esc hide-header-close
            v-model="showLogin"
            @ok="handleOk"
    >

        <b-form @submit.stop.prevent="handleSubmit">

            <b-form-group
                    id="fieldset-login"
                    :state="validateState('login')"
            >
                <b-form-input
                        id="input-login"
                        type="text"
                        v-model.trim="$v.formData.login.$model"
                        :state="validateState('login')"
                        placeholder="Логин"
                >
                </b-form-input>
                <b-form-invalid-feedback id="input-login-invalid-feedback">
                    Поле "Логин" не должно быть пустым
                </b-form-invalid-feedback>
            </b-form-group>

            <b-form-group
                    id="fieldset-password"
                    :state="validateState('password')"
            >
                <b-input
                        id="input-password"
                        type="password"
                        v-model.trim="$v.formData.password.$model"
                        :state="validateState('password')"
                        placeholder="Пароль"
                >
                </b-input>
                <b-form-invalid-feedback id="input-password-invalid-feedback">
                    Поле "Пароль" не должно быть пустым
                </b-form-invalid-feedback>
            </b-form-group>

        </b-form>


        <template v-slot:modal-footer="{ok}">

            <b-button @click="ok()" type="submit" variant="primary">Войти</b-button>

            <b-overlay variant="light" opacity=".30" :show="showLoader" no-wrap></b-overlay>

        </template>

    </b-modal>

</template>

<script>
    import {required} from 'vuelidate/lib/validators'

    export default {
        name: "FormLogin",
        data() {
            return {
                showLogin: true,
                showLoader: false,
                formData: {
                    login: null,
                    password: null,
                },
            }
        },
        validations: {
            formData: {
                login: {
                    required,
                },
                password: {
                    required,
                },
            },
        },
        mounted() {
            this.showLogin = true
        },
        methods: {
            validateState(name) {
                const { $dirty, $error } = this.$v.formData[name]
                return $dirty ? !$error : null
            },
            resetForm() {
                this.formData = {
                    login: null,
                    password: null,
                }
                this.$nextTick(() => {
                    this.$v.$reset()
                })
            },
            handleOk(bvModalEvt) {
                // Prevent modal from closing
                bvModalEvt.preventDefault()
                // Trigger submit handler
                this.handleSubmit()
            },
            handleSubmit() {
                this.$v.formData.$touch()
                if (this.$v.formData.$anyError) {
                    return
                }
                this.$emit('submited', this.formData)
                this.showLoader = true
                setTimeout(() => {
                    this.resetForm()
                    this.showLoader = false
                }, 2000)
            },
        },
    }
</script>