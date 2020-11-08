<template>
  <v-container fluid class="d-flex flex-column justify-center pa-0">
    <v-form ref="form" v-model="valid" lazy-validation>
      <v-card elevation="12" class="pa-0" color="grey darken-4" dark>
        <v-card-title class="color--card justify-center">
          <h1
            class="display-1 font-weight-medium text--yellow--accent text-center"
          >
            Add Item
          </h1>
        </v-card-title>
        <v-card-text>
          <v-text-field
            label="What are you going to do?"
            outlined
            :rules="rules"
            v-model="todo.name"
          ></v-text-field>
        </v-card-text>
        <v-card-actions class="justify-center pt-0">
          <v-btn outlined color="#f7ef64" @click="addItem">Add To List</v-btn>
        </v-card-actions>
      </v-card>
    </v-form>
  </v-container>
</template>

<script>
import moment from "moment";

export default {
  name: "AddToDo",
  props: ["selectedDate"],
  data: () => ({
    valid: false,
    rules: [
      (value) => !!value || "Required.",
      (value) => (value || "").length > 5 || "At least 5 characters",
    ],
    todo: {
      date: null,
      name: "",
      status: false,
    },
  }),

  methods: {
    validate() {
      return this.$refs.form.validate();
    },

    resetValidation() {
      this.$refs.form.resetValidation();
    },

    clearTodo() {
      this.todo.date = moment().format("YYYY-MM-DD");
      this.todo.name = "";
    },

    addItem() {
      if (this.validate()) {
        this.todo.date = this.selectedDate;
        this.$emit("clicked", this.todo);
        this.clearTodo();
        this.resetValidation();
      }
    },
  },

  computed: {
    getDate() {
      return this.selectedDate;
    },
  },
};
</script>
