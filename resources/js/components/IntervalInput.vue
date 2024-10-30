<template>
    <div class="interval">
        <time-input
            :time-prop="from"
            :use-text-inputs="useTextInputs"
            @blur="updateInterval"
            v-model="from"
        />
        -
        <time-input
            :time-prop="to"
            :use-text-inputs="useTextInputs"
            @blur="updateInterval"
            v-model="to"
        />
        <span class="ml-2">
            <remove-button @click.prevent="$emit('removeInterval')" />
        </span>
    </div>
</template>

<script>
import { useTextInputsProp } from "../src/props";
import RemoveButton from './RemoveButton';
import TimeInput from "./TimeInput";

export default {
    components: { RemoveButton, TimeInput },

    props: {
        intervalProp: String,
        ...useTextInputsProp,
    },

    emits: ['updateInterval', 'removeInterval'],

    data() {
        let from = '09:00'; // default start time
        let to = '20:00';   // default end time

        // Parse intervalProp if it's in the "start-end" format
        if (this.intervalProp && typeof this.intervalProp === 'string' && this.intervalProp.includes('-')) {
            const [start, end] = this.intervalProp.split('-');
            from = start;
            to = end;
        }

        return {
            from,
            to
        };
    },

    computed: {
        interval() {
            return `${this.from}-${this.to}`;
        }
    },

    methods: {
        updateInterval() {
            this.$emit('updateInterval', this.interval);
        }
    },

    watch: {
        interval(newInterval) {
            this.$emit('updateInterval', newInterval);
        }
    }
}
</script>

<style scoped>
.interval {
    margin: 10px 0;
}
</style>
