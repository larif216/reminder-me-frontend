<template>
    <b-button class="btn-danger" size="sm" v-on:click="triggered(id)">
        Hapus
    </b-button>
</template>

<script>

export default {
    name: 'ButtonDelete',
    data() {
        return {
        }
    },
    props: {
        id : {
            type: Number,
            default: null,
            required: true
        }
    },
    methods: {
        triggered(id) {
            this.$emit('deleteData', id)
        }
    }
}
</script>