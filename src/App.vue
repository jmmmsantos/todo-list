<template>
  <v-app>
    <v-main class="color--bg d-flex flex-row">
      <v-container fluid>
        <v-row>
          <v-col cols="12" sm="3">
            <v-sheet rounded="lg" min-height="268">
              <v-date-picker
                v-model="selectedDate"
                full-width
                elevation="12"
                color="#65A9C0"
              ></v-date-picker>
            </v-sheet>
          </v-col>

          <v-col cols="12" sm="6">
            <v-sheet min-height="70vh" rounded="lg">
              <!--  -->
              <ViewToDo v-bind:list="selectedList" @clicked="onDeleteToDo" />
            </v-sheet>
          </v-col>

          <v-col cols="12" sm="3">
            <v-sheet rounded="lg" min-height="268">
              <!--  -->
              <AddToDo
                @clicked="onAddToDoList"
                v-bind:selectedDate="selectedDate"
              />
            </v-sheet>
          </v-col>
        </v-row>
      </v-container>
    </v-main>
  </v-app>
</template>

<script>
import AddToDo from "./components/AddToDo";
import ViewToDo from "./components/ViewToDo";
import moment from "moment";

export default {
  name: "App",

  components: {
    AddToDo,
    ViewToDo,
  },

  data: () => ({
    //
    selectedDate: null,
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
  },

  mounted() {
    this.selectedDate = moment().format("YYYY-MM-DD");
  },
};
</script>

<style>
.color--bg {
  background-color: #f1ede8;
}

.color--card {
  background-color: #65a9c0;
}

.container {
  -webkit-user-select: none;
  -moz-user-select: none;
  -ms-user-select: none;
  -o-user-select: none;
  user-select: none;
}
</style>
