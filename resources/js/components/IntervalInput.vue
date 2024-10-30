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
        let intervalString = this.intervalProp;
        console.log(intervalString);

        // If intervalProp is an object, access its `interval` property
        if (typeof this.intervalProp === 'object' && this.intervalProp !== null) {
            intervalString = this.intervalProp.interval;
        }

        // Split the interval string to get `from` and `to` times
        const [from, to] = (intervalString || '09:00-20:00').split('-');

        return {
            interval: intervalString,
            from,
            to
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