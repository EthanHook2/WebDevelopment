<template>
    <div>
        <Counter :year="2024" :month="1" :date="1" :hour="0" :minute="0" :second="0" :millisecond="0" />
    </div>
</template>

<script>
    import Counter from '@/components/Counter.vue'

    export default {
        components: {
            Counter
        }
    }
</script>
