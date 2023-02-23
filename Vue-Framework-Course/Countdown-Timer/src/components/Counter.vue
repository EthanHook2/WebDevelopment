<template>
    <div v-if="loaded">
        <section  class="text-4xl flex justify-center content-center flex-col mx-auto text-center">
            <h3 v-if="!expired">Counting Down </h3>
            <h3 v-else>Coundown Completed</h3>
        </section>
        <section class="flex text-6xl justify-center content-center">
            <div class="days relative mr-2">
                {{ displayDays }}
                <div class="label text-sm absolute bottom-0">Days</div>
            </div>
            <span class="leading-snug">:</span>
            <div class="hours mx-2 relative">
                {{ displayHours }}
                <div class="label text-sm absoulute bottom-0">Hours</div>
            </div>
            <span class="leading-snug">:</span>
            <div class="hours mx-2 relative">
                {{ displayMinutes }}
                <div class="label text-sm absoulute bottom-0">Minutes</div>
            </div>
            <span class="leading-snug">:</span>
            <div class="seconds ml-2 relative">
                {{ displaySeconds }}
                <div class="label text-sm absoulute bottom-0">Seconds</div>
            </div>
        </section>
    </div>
</template>

<script>
export default {
    props: ['year', 'month', 'date', 'hour', 'minute',  'second', 'millisecond'],

    data: () => ({
        displayDays: 0,
        displayHours: 0,
        displayMinutes: 0,
        displaySeconds: 0,
        loaded: false,
        expired: false
    }),
    computed: {
        _seconds: () => 1000,
        _minutes() {
            return this._seconds * 60;
        },
        _hours() {
            return this._minutes * 60;
        },
        _days() {
            return this._hours * 24;
        },
        end() {
            return new Date(
                this.year, 
                this.month, 
                this.date, 
                this.hour, 
                this.minute, 
                this.second,
                this.millisecond
            );
        }
    },
    mounted() {
        this.showRemaining();
    },
    methods: {
        formatNum: nun => (nun < 10 ? "0" + nun : nun),

        showRemaining() {
            const timer = setInterval(() => {
                const now = new Date();
               // const end = new Date(2023, 9, 14, 12, 12, 12, 12);
                const distance = this.end.getTime() - now.getTime();

                if (distance < 0) {
                    clearInterval(timer);
                    this.expired = true;
                    return;
                }

                const days = Math.floor(distance / this._days);
                const hours = Math.floor((distance % this._days) / this._hours);
                const minutes = Math.floor((distance % this._hours) / this._minutes);
                const seconds = Math.floor((distance % this._minutes) / this._seconds);

                this.displayMinutes = this.formatNum(minutes);
                this.displaySeconds = this.formatNum(seconds);
                this.displayHours = this.formatNum(hours);
                this.displayDays = this.formatNum(days);
                this.loaded = true;

            }, 1000);
        }
    }
};
</script>

<style scoped>
.seconds {
    max-width: 60px;
}
</style>
