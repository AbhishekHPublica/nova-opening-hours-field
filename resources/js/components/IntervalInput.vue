<template>
    <div class="interval">
        <time-input
            :time-prop="from"
            :use-text-inputs="useTextInputs"
            @blur="this.interval = [this.from, this.to].join('-');"
            v-model="from"
        />
        -
        <time-input
            :time-prop="to"
            :use-text-inputs="useTextInputs"
            @blur="this.interval = [this.from, this.to].join('-');"
            v-model="to"
        />
        <span class="ml-2">
            <remove-button @click.prevent="$emit('removeInterval')" />
        </span>
    </div>
</template>

<script>
import {useTextInputsProp} from "../src/props";
import RemoveButton from './RemoveButton';
import TimeInput from "./TimeInput";

export default {
    components: { RemoveButton, TimeInput},

    props:  {
        intervalProp: String,
        ...useTextInputsProp,
    },

    emits: ['updateInterval', 'removeInterval'],

    data: function () {
        let from = '09:00'; // default start time
        let to = '20:00';   // default end time
        console.log(this.intervalProp);
        console.log(typeof this.intervalProp);
        console.log(typeof this.intervalProp.interval === 'string');
        console.log(this.intervalProp.interval.includes('-'));
        if (this.intervalProp && this.intervalProp?.interval && typeof this.intervalProp.interval === 'string' && this.intervalProp.interval.includes('-')) {
            const [start, end] = this.intervalProp.interval.split('-');
            from = start;
            to = end;
        }

        return {
            interval: this.intervalProp,
            from: from,
            to: to
        };
    },


    watch: {
        interval(value) {
            // if (value.length !== 11 || value === this.interval) return
            this.$emit('updateInterval', value)
        },
    },
}
</script>

<style scoped>
.interval {
    margin: 10px 0;
}
</style>
