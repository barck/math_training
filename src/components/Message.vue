<template>
    <div class="alert alert-primary" :class="'alert-'+ type">
        <h2>{{ text }}</h2>
        <hr>
        <button class="btn btn-lg btn-primary" @click="$emit('onNext')">
            Continue
        </button>
    </div>
</template>

<script>
    export default {
        props: ['type', 'text']
    }
</script>

<style>

</style>