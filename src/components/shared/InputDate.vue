<template>
    <v-row>
        <v-col cols="12">
            <v-menu
                ref="menu1"
                v-model="menu1"
                :close-on-content-click="false"
                transition="scale-transition"
                offset-y
                max-width="290px"
                min-width="auto"
            >
                <template #activator="{ on, attrs }">
                    <v-text-field
                        v-model="dateFormatted"
                        label="Data"
                        hint="MM/DD/YYYY"
                        outlined
                        dense
                        prepend-icon="mdi-calendar"
                        v-bind="attrs"
                        background-color="white"
                        color="black"
                        @blur="date = parseDate(dateFormatted)"
                        v-on="on"
                    />
                </template>
                <v-date-picker v-model="date" no-title locale="pt-br" @input="menu1 = false" />
            </v-menu>
        </v-col>
    </v-row>
</template>

<script>
export default {
    name: 'InputDate',

    props: {
        label: {
            type: String,
            default: 'Data',
        },
        dataEnviada: {
            type: String,
            default: '',
        },
    },
    data() {
        return {
            date: '',
            dateFormatted: '',
            menu1: false,
        };
    },

    computed: {
        computedDateFormatted() {
            return this.formatDate(this.date);
        },
    },

    watch: {
        date() {
            this.dateFormatted = this.formatDate(this.date);
        },

        dateFormatted(novaData) {
            this.$emit('input', novaData);
        },
    },

    methods: {
        formatDate(date) {
            if (!date) return null;

            const [year, month, day] = date.split('-');
            return `${day}/${month}/${year}`;
        },
        parseDate(date) {
            if (!date) return null;

            const [day, month, year] = date.split('/');
            return `${year}-${month.padStart(2, '0')}-${day.padStart(2, '0')}`;
        },
    },
};
</script>

<style scoped></style>
