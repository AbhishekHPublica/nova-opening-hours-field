<template>
    <div>
        <table class="openingHours weekTable table-default w-full">
            <thead class="bg-gray-50 dark:bg-gray-800">
                <tr>
                    <table-header :colspan="editable ? 3 : 2">
                    {{ __('Week') }}
                    </table-header>
                </tr>
            </thead>
            <tbody>
                <tr v-for="(day, index) in week" :key="day.day" class="group">
                    <table-column>
                        {{ __(capitalizeFirstLetter(day.day)) }}
                    </table-column>
                    <table-column>
                        <div v-if="day.intervals.length">
                            <div v-for="(interval, intervalIndex) in day.intervals" :key="intervalIndex">
                                <interval-input
                                    :interval-prop="interval"
                                    :use-text-inputs="useTextInputs"
                                    @updateInterval="$emit('updateInterval', 'week', day.day, intervalIndex, $event)"
                                    @removeInterval="$emit('removeInterval', 'week', day.day, intervalIndex)"
                                />
                            </div>
                        </div>
                        <div v-else :class="{'closed': editable}">
                            {{ __('Closed') }}
                        </div>
                    </table-column>
                    <table-column v-if="editable" class="text-right">
                        <add-button @click.prevent="addInterval(index, day.day)" />
                        <span v-if="day.intervals.length" class="ml-2">
                            <remove-button @click.prevent="$emit('removeAllIntervals', 'week', day.day)" />
                        </span>
                    </table-column>
                </tr>
            </tbody>
        </table>
    </div>
</template>
  
<script>
export default {
    props: {
        week: {
            type: Array,
            default: () => ([
            { day: 'monday', intervals: [] },
            { day: 'tuesday', intervals: [] },
            { day: 'wednesday', intervals: [] },
            { day: 'thursday', intervals: [] },
            { day: 'friday', intervals: [] },
            { day: 'saturday', intervals: [] },
            { day: 'sunday', intervals: [] },
            ]),
        },
        editable: Boolean,
        useTextInputs: Boolean,
    },
    methods: {
        addInterval(index, day) {
            const previousDayIndex = index > 0 ? index - 1 : null;
            let previousDayIntervals = null;
    
            if (previousDayIndex !== null) {
                previousDayIntervals = this.week[previousDayIndex].intervals;
            }
            const defaultStart = '09:00';
            const defaultEnd = '20:00';
    
            const previousInterval = previousDayIntervals && previousDayIntervals.length
            ? previousDayIntervals[previousDayIntervals.length - 1]
            : { start: defaultStart, end: defaultEnd };
            this.week[index].intervals.push({
                start: previousInterval.start,
                end: previousInterval.end,
            });
        },
        },
};
</script>  