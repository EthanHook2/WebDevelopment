<template>
    <div class="grid md:grid-cols-2 gap-4">
        <!-- Cases -->
        <div class="shadow-md bg-blue-300 p-10 text-center rounded">
            <h3 class="text-3xl font-extrabold text-black mb-4">Cases</h3>

            <div class="text-2xl mb-4">
                <span class="font-bold">
                    New:
                </span>
                {{ numberWithCommas(stats.NewConfirmed )}}
            </div>
            <div class="text-2xl mb-4">
                <span class="font-bold">
                    Total:
                </span>
                {{ numberWithCommas(stats.TotalConfirmed )}}
            </div>
        </div>
        <!-- Deaths -->
        <div class="shadow-md bg-blue-300 p-10 text-center rounded">
            <h3 class="text-3xl font-extrabold text-black mb-4">Deaths</h3>

            <div class="text-2xl mb-4">
                <span class="font-bold">
                    New:
                </span>
                {{ numberWithCommas(stats.NewDeaths )}}
            </div>
            <div class="text-2xl mb-4">
                <span class="font-bold">
                    Total:
                </span>
                {{ numberWithCommas(stats.TotalDeaths )}}
            </div>
        </div>
    </div>
</template>

<script>
    export default {
        name: 'DataBoxes',
        props: ['stats'],
        methods: {
            numberWithCommas(x) {
                return x.toString().replace(/\B(?=(\d{3})+(?!\d))/g, ',')
            }
        }
    }
</script>