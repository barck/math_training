<template>
    <div class="alert alert-primary">
        <h3>Are you ready?</h3>
        <button class="btn btn-lg btn-success" @click="$emit('onStart')">Start</button> <!--генератор события onStart -->
    </div>
</template>

<script>
export default {
    methods: {
        some() {
            alert('Some here')
        }
    }
}
</script>

<style lang="scss" scoped>

</style>