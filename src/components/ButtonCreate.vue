<template>
    <b-button class="btn-success" size="sm" v-on:click="triggered()">
        Tambah Deadline
    </b-button>
</template>

<script>

export default {
    name: 'ButtonCreate',
    data() {
        return {
        }
    },
    methods: {
        triggered() {
            this.$emit('showModalCreate', true)
        }
    }
}
</script>