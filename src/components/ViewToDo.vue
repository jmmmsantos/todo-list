<template>
  <v-container fluid class="pa-0">
    <v-card
      flat
      tile
      class="pa-0 overflow-hidden"
      max-height="90vh"
      min-height="90vh"
      color="grey darken-4"
    >
      <v-card-title class="color--card justify-center">
        <h1
          class="display-1 font-weight-medium text--yellow--accent text-center"
        >
          Activities
        </h1>
      </v-card-title>
      <v-sheet
        v-if="list[0] !== undefined"
        class="overflow-y-auto"
        max-height="79vh"
        color="grey darken-4"
      >
        <v-card
          v-for="(item, i) in list"
          :key="'item_' + i"
          class="ma-5"
          elevation="12"
          dark
        >
          <v-card-title class="color--list justify-space-between">
            <v-btn light icon @click="previousDay()">
              <v-icon color="grey darken-4">{{ mdiChevronLeft }}</v-icon>
            </v-btn>
            <h1 class="body-1 text-center text--grey font-weight-bold">
              {{ formattedDate(item.date) }}
            </h1>
            <v-btn light icon @click="nextDay()">
              <v-icon color="grey darken-4">{{ mdiChevronRight }}</v-icon>
            </v-btn>
          </v-card-title>
          <v-card-text class="overflow-x-hidden">
            <template v-for="(task, i) in item.list">
              <v-divider v-if="i !== 0" :key="`${i}-divider`" dark></v-divider>

              <v-list-item :key="`${i}-${task.name}`">
                <v-list-item-action>
                  <v-checkbox
                    v-model="task.status"
                    :color="(task.status && 'success') || 'primary'"
                    class="text-break text"
                  >
                    <template v-slot:label>
                      <div
                        :class="
                          (task.status &&
                            'grey--text text-decoration-line-through') ||
                            'white--text'
                        "
                        class="ml-4"
                        v-text="task.name"
                      ></div>
                    </template>
                  </v-checkbox>
                </v-list-item-action>

                <v-spacer></v-spacer>

                <v-scroll-x-transition>
                  <v-btn icon @click="deleteItem(task, item.date)">
                    <v-icon color="error">{{ mdiWindowClose }}</v-icon>
                  </v-btn>
                </v-scroll-x-transition>
              </v-list-item>
            </template>
          </v-card-text>
        </v-card>
      </v-sheet>
      <v-sheet
        v-if="list[0] === undefined"
        class="overflow-y-auto"
        max-height="600"
        color="grey darken-4"
      >
        <v-card class="ma-5">
          <v-card-title class="color--list d-flex justify-space-between">
            <v-btn light icon @click="previousDay()">
              <v-icon color="grey darken-4">{{ mdiChevronLeft }}</v-icon>
            </v-btn>
            <h1 class="body-1 text-center text--grey font-weight-bold">
              {{ getDate }}
            </h1>
            <v-btn light icon @click="nextDay()">
              <v-icon color="grey darken-4">{{ mdiChevronRight }}</v-icon>
            </v-btn>
          </v-card-title>
          <v-card-text class="color--bg d-flex justify-center">
            <h1 class="body-1 font-weight-light white--text text-break py-5">
              List for this day is empty. Add tasks now!
            </h1>
          </v-card-text>
        </v-card>
      </v-sheet>
    </v-card>
  </v-container>
</template>

<script>
import {
  mdiWindowClose,
  mdiCheck,
  mdiChevronLeft,
  mdiChevronRight,
} from "@mdi/js";
import moment from "moment";

export default {
  name: "ViewToDo",
  props: {
    list: {
      type: Array,
    },
    selectedDate: {
      type: String,
    },
  },

  data: () => ({
    mdiWindowClose,
    mdiCheck,
    mdiChevronLeft,
    mdiChevronRight,
  }),

  methods: {
    nextDay() {
      const nextDay = moment(this.selectedDate)
        .add(1, "days")
        .format("YYYY-MM-DD");
      this.$emit("next", nextDay);
    },

    previousDay() {
      const previousDay = moment(this.selectedDate)
        .subtract(1, "days")
        .format("YYYY-MM-DD");
      this.$emit("previous", previousDay);
    },

    formattedDate(date) {
      return new moment(date).format("ddd, MMM. DD, YYYY");
    },

    deleteItem(item, date) {
      this.$emit("clicked", item, date);
    },
  },

  computed: {
    getDate() {
      return this.formattedDate(this.selectedDate);
    },
  },
};
</script>
