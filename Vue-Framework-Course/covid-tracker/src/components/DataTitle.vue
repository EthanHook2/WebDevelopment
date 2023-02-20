<template>
    <div class="text-center">
        <h2 class="text-5xl font-extrabold">{{ text }}</h2>
        <div class="text-3xl mt-4 mb-10 font-bold">{{ timestamp }}</div>
    </div>
</template>

<script>
    import moment from 'moment'

    export default {
        name: 'DataTitle',
        props: ['text', 'dateDate'],
        computed: {
            timestamp: function() {
                return moment(this.dateDate).format('MMMM Do YYYY, h:mm:ss a')
            }
        }
    }
</script>