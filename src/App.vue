<template>
    <div id="app">
        <FormLogin @submited="onFormLogin"/>
    </div>
</template>

<script>
    import FormLogin from "./components/FormLogin"

    export default {
        methods: {
            onFormLogin(formData) {
                console.log('Submited gata:', formData)
            },
        },
        components: {
            FormLogin,
        },
    }
</script>

