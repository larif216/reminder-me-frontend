<template>
    <b-button class="btn-warning" size="sm" v-on:click="triggered(id)">
        Ubah
    </b-button>
</template>

<script>

export default {
    name: 'ButtonUpdate',
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
            this.$emit('showModalUpdate', id)
        }
    }
}
</script>