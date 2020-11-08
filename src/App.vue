<template>
  <v-app>
    <v-overlay class="d-flex flex-column" :opacity="1" :value="overlay">
      <div class="d-flex justify-center">
        <v-progress-circular
          indeterminate
          color="#f7ef64"
          size="64"
        ></v-progress-circular>
      </div>
      <h1 class="font-weight-light headline py-5 white--text">
        Now Loading...
      </h1>
    </v-overlay>
    <v-main class="color--bg d-flex flex-row">
      <v-dialog
        v-if="$vuetify.breakpoint.smAndDown"
        v-model="instructionDialog"
        persistent
        width="100vw"
        class="mx-0"
      >
        <v-container class="d-flex flex-column px-0">
          <div class="d-flex flex-row justify-center align-center">
            <v-icon color="#f7ef64" size="28" class="pl-1">{{
              mdiInformation
            }}</v-icon>
            <h1 class="text--yellow--accent title text-center ml-1">
              Basic Instructions
            </h1>
          </div>
          <Instructions @close="closeInstructions" />
        </v-container>
      </v-dialog>
      <v-container
        fluid
        class="pa-0"
        v-if="$vuetify.breakpoint.smAndDown && !instructionDialog"
      >
        <ViewToDo
          v-bind:selectedDate="selectedDate"
          v-bind:list="selectedList"
          @clicked="onDeleteToDo"
          @next="nextDate"
          @previous="previousDate"
        />
        <v-menu
          width="100vw"
          min-width="100vw"
          bottom
          :close-on-content-click="addToDoMenu"
          :offset-y="true"
          nudge-bottom="39vh"
          class="d-flex justify-center"
        >
          <template v-slot:activator="{ on, attrs }">
            <v-btn
              color="#f7ef64"
              dark
              absolute
              top
              right
              icon
              outlined
              v-bind="attrs"
              v-on="on"
              @click="addToDoMenu = false"
            >
              <v-icon>{{ mdiPlus }}</v-icon>
            </v-btn>
          </template>
          <AddToDo
            @clicked="onAddToDoList"
            @close="closeAddToDo"
            v-bind:selectedDate="selectedDate"
          />
        </v-menu>
        <v-menu
          min-width="100vw"
          bottom
          :close-on-content-click="false"
          :offset-y="true"
        >
          <template v-slot:activator="{ on, attrs }">
            <v-btn
              color="#f7ef64"
              dark
              absolute
              top
              left
              icon
              v-bind="attrs"
              v-on="on"
            >
              <v-icon size="25">{{ mdiCalendarCheck }}</v-icon>
            </v-btn>
          </template>

          <v-date-picker
            v-model="selectedDate"
            full-width
            dark
            header-color="grey darken-4"
            elevation="12"
            color="#f7ef64"
          ></v-date-picker>
        </v-menu>
      </v-container>
      <v-container fluid v-if="$vuetify.breakpoint.mdAndUp">
        <v-dialog v-model="instructionDialog" persistent width="70vw">
          <v-container class="d-flex flex-column">
            <div class="d-flex flex-row justify-center align-center">
              <v-icon color="#f7ef64" size="45" class="pr-1">{{
                mdiInformation
              }}</v-icon>
              <h1 class="text--yellow--accent display-1 text-center pl-1">
                Basic Instructions
              </h1>
            </div>
            <Instructions @close="closeInstructions" />
          </v-container>
        </v-dialog>
        <v-row>
          <v-col cols="12" sm="3">
            <v-date-picker
              v-model="selectedDate"
              full-width
              dark
              header-color="grey darken-4"
              elevation="12"
              class="mb-3"
              color="#f7ef64"
            ></v-date-picker>
            <AddToDo
              @clicked="onAddToDoList"
              v-bind:selectedDate="selectedDate"
            />
          </v-col>

          <v-col cols="12" sm="9">
            <v-sheet min-height="70vh" rounded="lg">
              <!--  -->
              <ViewToDo
                v-bind:selectedDate="selectedDate"
                v-bind:list="selectedList"
                @clicked="onDeleteToDo"
                @next="nextDate"
                @previous="previousDate"
              />
            </v-sheet>
          </v-col>
        </v-row>
      </v-container>
    </v-main>
  </v-app>
</template>

<script>
import { mdiPlus, mdiCalendarCheck, mdiInformation } from "@mdi/js";
import AddToDo from "./components/AddToDo";
import ViewToDo from "./components/ViewToDo";
import Instructions from "./components/Instructions";
import moment from "moment";

export default {
  name: "App",

  components: {
    AddToDo,
    ViewToDo,
    Instructions,
  },

  data: () => ({
    //
    addToDoMenu: false,
    instructionDialog: false,
    selectedDate: null,
    mdiPlus,
    mdiCalendarCheck,
    mdiInformation,
    overlay: true,
    list: [
      {
        date: moment().format("YYYY-MM-DD"),
        list: [
          {
            name: "Clean the house.",
            status: false,
          },
          {
            name: "Buy groceries.",
            status: false,
          },
        ],
      },
    ],
  }),

  computed: {
    selectedList() {
      return [this.list.find((day) => day.date === this.selectedDate)];
    },
  },

  methods: {
    onAddToDoList(todo) {
      let doesDateExist = this.list.find((day) => day.date === todo.date);
      if (doesDateExist) {
        this.list.find((day) => {
          if (day === doesDateExist) {
            day.list.push({
              name: todo.name,
              status: todo.status,
            });
          }
        });
      } else {
        this.list.push({
          date: todo.date,
          list: [
            {
              name: todo.name,
              status: todo.status,
            },
          ],
        });
      }
      this.list.sort(
        (a, b) =>
          new moment(a.date).format("YYYYMMDD") -
          new moment(b.date).format("YYYYMMDD")
      );
      if (this.$vuetify.breakpoint.smAndDown) {
        window.scrollTo({ top: 0, behavior: "smooth" });
      }
    },

    onDeleteToDo(item, date) {
      this.list.find((day) => {
        if (day.date === date) {
          day.list.splice(
            day.list.findIndex((entry) => entry.name === item.name),
            1
          );
          if (day.list.length === 0) {
            this.list.splice(
              this.list.findIndex((entry) => entry.date === date),
              1
            );
          }
        }
      });
    },

    nextDate(date) {
      this.selectedDate = date;
    },

    previousDate(date) {
      this.selectedDate = date;
    },

    closeInstructions() {
      this.instructionDialog = false;
      document.getElementsByTagName("html")[0].style.overflowY = null;
      window.scrollTo(0, 0);
    },

    closeAddToDo(state) {
      this.addToDoMenu = state;
    },

    initialize() {
      document.title = "To Do List App";
      document.getElementsByTagName("html")[0].style.overflowY = "hidden";
      window.setTimeout(() => {
        this.overlay = false;
      }, 5000);
      window.setTimeout(() => {
        this.instructionDialog = true;
      }, 5300);
    },
  },

  mounted() {
    this.initialize();
    this.selectedDate = moment().format("YYYY-MM-DD");
  },
};
</script>

<style>
.text--yellow--accent {
  color: #f7ef64 !important;
}

.text--grey {
  color: #212121 !important;
}

div.theme--dark.v-picker__body {
  background-color: #212121;
}

.v-picker__title__btn--active {
  color: #f7ef64;
}

.v-btn--active div.v-btn__content {
  color: #212121;
}

.color--bg {
  background-color: #212121;
}

.color--card {
  background-color: #212121;
}

.color--list {
  background-color: #f7ef64;
}

.v-stepper__content,
.v-stepper__header,
.v-stepper__items {
  background-color: #212121;
}

.v-stepper--vertical {
  padding-bottom: 0;
}

.v-stepper {
  box-shadow: none !important;
}

.theme--dark.v-stepper .v-stepper__step__step .v-icon,
.theme--dark.v-stepper .v-stepper__step__step {
  color: #212121 !important;
}

html,
.container {
  -webkit-user-select: none;
  -moz-user-select: none;
  -ms-user-select: none;
  -o-user-select: none;
  user-select: none;
  background-color: #212121;
}
</style>
