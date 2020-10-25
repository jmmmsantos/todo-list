<template>
  <v-container fluid class="pa-0">
    <v-card
      elevation="12"
      class="pa-0 overflow-hidden"
      max-height="90vh"
      min-height="90vh"
    >
      <v-card-title class="color--card justify-center">
        <h1 class="display-1 font-weight-medium white--text text-center">
          To Do List
        </h1>
      </v-card-title>
      <v-sheet
        v-if="list[0] !== undefined"
        id="scrolling-techniques-6"
        class="overflow-y-auto"
        max-height="600"
      >
        <v-card v-for="item in list" :key="item" class="ma-5">
          <v-card-title class="color--card justify-center">
            <h1 class="body-1 text-center white--text font-weight-bold">
              {{ formattedDate(item.date) }}
            </h1>
          </v-card-title>
          <v-card-text class="overflow-x-hidden">
            <v-slide-y-transition class="py-0" group tag="v-list">
              <template v-for="(task, i) in item.list">
                <v-divider v-if="i !== 0" :key="`${i}-divider`"></v-divider>

                <v-list-item :key="`${i}-${task.name}`">
                  <v-list-item-action>
                    <v-checkbox
                      v-model="task.status"
                      :color="(task.status && 'grey') || 'primary'"
                      class="text-truncate"
                    >
                      <template v-slot:label>
                        <div
                          :class="
                            (task.status && 'grey--text') || 'black--text'
                          "
                          class="ml-4"
                          v-text="task.name"
                        ></div>
                      </template>
                    </v-checkbox>
                  </v-list-item-action>

                  <v-spacer></v-spacer>

                  <v-scroll-x-transition>
                    <v-icon v-if="task.status" color="success">
                      mdi-check
                    </v-icon>
                    <v-btn v-else icon @click="deleteItem(task, item.date)">
                      <v-icon color="error">mdi-window-close</v-icon>
                    </v-btn>
                  </v-scroll-x-transition>
                </v-list-item>
              </template>
            </v-slide-y-transition>
          </v-card-text>
        </v-card>
      </v-sheet>
      <v-sheet
        v-if="list[0] === undefined"
        id="scrolling-techniques-6"
        class="overflow-y-auto"
        max-height="600"
      >
        <v-card class="ma-5">
          <v-card-title>
            <h1 class="body-1 font-weight-light">
              To do list for this day is empty. Add one now!
            </h1>
          </v-card-title>
        </v-card>
      </v-sheet>
    </v-card>
  </v-container>
</template>

<script>
import moment from "moment";

export default {
  name: "ViewToDo",
  props: {
    list: {
      type: Array,
    },
  },

  methods: {
    formattedDate(date) {
      return new moment(date).format("ddd, MMM. DD, YYYY");
    },

    deleteItem(item, date) {
      this.$emit("clicked", item, date);
    },
  },
};
</script>
