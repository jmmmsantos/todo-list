<template>
  <v-stepper
    v-model="instructionTabMobile"
    vertical
    v-if="$vuetify.breakpoint.smAndDown"
    dark
  >
    <v-container class="pa-0 pb-5">
      <template v-for="(step, i) in steps">
        <v-stepper-step
          @click="instructionTabMobile = i + 1"
          :key="'step_mobile_' + i"
          :complete="instructionTabMobile > i"
          color="#f7ef64"
          :step="i + 1"
        >
          {{ step.label }}
        </v-stepper-step>

        <v-stepper-content :key="'step_mobile_content_' + i" :step="i + 1">
          <v-card flat tile color="grey darken-4" class="mb-5">
            <v-img
              aspect-ratio="1"
              contain
              :key="'step_mobile_img_' + i"
              :src="step.image_mobile"
              min-height="150px"
              height="15vh"
            >
              <template v-slot:placeholder>
                <v-row class="fill-height ma-0" align="center" justify="center">
                  <v-progress-circular
                    indeterminate
                    color="#f7ef64"
                  ></v-progress-circular>
                </v-row>
              </template>
            </v-img>
            <h1 class="subtitle-1 font-weight-light text-justify mt-5">
              {{ step.description_mobile }}
            </h1>
          </v-card>
          <div class="d-flex justify-center">
            <v-btn
              color="#f7ef64"
              :key="'step_previous_' + i"
              class="mr-2"
              :disabled="i < 1"
              outlined
              dark
              icon
              @click="instructionTabMobile = instructionTabMobile - 1"
            >
              <v-icon>{{ mdiChevronUp }}</v-icon>
            </v-btn>
            <v-btn
              color="#f7ef64"
              :key="'step_next_' + i"
              :disabled="i > 1"
              outlined
              dark
              icon
              @click="instructionTabMobile = instructionTabMobile + 1"
            >
              <v-icon>{{ mdiChevronDown }}</v-icon>
            </v-btn>
            <v-spacer></v-spacer>
            <v-btn v-if="i === 2" text color="#f7ef64" @click="$emit('close')"
              >Got It!</v-btn
            >
          </div>
        </v-stepper-content>
      </template>
    </v-container>
  </v-stepper>
  <v-stepper v-model="instructionTab" dark v-else>
    <v-stepper-header>
      <template v-for="(step, i) in steps">
        <v-stepper-step
          @click="instructionTab = i + 1"
          :key="'step_' + i"
          color="#f7ef64"
          :complete="instructionTab > i"
          :step="i + 1"
        >
          {{ step.label }}
        </v-stepper-step>

        <v-divider :key="'divider_' + i" v-if="i < 2"></v-divider>
      </template>
    </v-stepper-header>

    <v-stepper-items>
      <template v-for="(content, i) in steps">
        <v-stepper-content :key="'step_content_' + i" :step="i + 1">
          <v-card
            class="mb-16"
            color="grey darken-4"
            min-height="300px"
            max-height="45vh"
            flat
            tile
          >
            <v-container
              fluid
              :key="'step_container_' + i"
              class="d-flex align-center"
              :class="i < 2 ? 'flex-row' : 'flex-column align-content-start'"
            >
              <v-img
                aspect-ratio="1"
                contain
                :key="'step_img_' + i"
                :src="content.image"
                :height="i < 2 ? '30vh' : '200px'"
                :width="i < 2 ? '300px' : '53.5vw'"
                :max-height="i < 2 ? '400px' : ''"
                :max-width="i < 2 ? '300px' : '53.5vw'"
                :min-height="i < 2 ? '280px' : ''"
                min-width="300px"
                :class="i < 2 && !$vuetify.breakpoint.mdAndDown ? 'ml-16' : ''"
              >
                <template v-slot:placeholder>
                  <v-row
                    class="fill-height ma-0"
                    align="center"
                    justify="center"
                  >
                    <v-progress-circular
                      indeterminate
                      color="#f7ef64"
                    ></v-progress-circular>
                  </v-row>
                </template>
              </v-img>
              <h1
                class="subtitle-1 text-justify"
                :class="$vuetify.breakpoint.mdAndDown ? 'mx-5' : 'mx-16'"
              >
                {{ content.description }}
              </h1>
            </v-container>
          </v-card>
          <div class="d-flex">
            <v-btn
              color="#f7ef64"
              :key="'step_previous_' + i"
              :disabled="i < 1"
              outlined
              dark
              icon
              class="mr-2"
              @click="instructionTab = instructionTab - 1"
            >
              <v-icon>{{ mdiChevronLeft }}</v-icon>
            </v-btn>
            <v-btn
              color="#f7ef64"
              :key="'step_next_' + i"
              :disabled="i > 1"
              outlined
              dark
              icon
              @click="instructionTab = instructionTab + 1"
            >
              <v-icon>{{ mdiChevronRight }}</v-icon>
            </v-btn>
            <v-spacer></v-spacer>
            <v-btn v-if="i === 2" text color="#f7ef64" @click="$emit('close')"
              >Got It!</v-btn
            >
          </div>
        </v-stepper-content>
      </template>
    </v-stepper-items>
  </v-stepper>
</template>

<script>
import {
  mdiChevronLeft,
  mdiChevronRight,
  mdiChevronUp,
  mdiChevronDown,
} from "@mdi/js";

export default {
  name: "Instructions",

  data: () => ({
    instructionTab: 1,
    instructionTabMobile: 1,
    mdiChevronLeft,
    mdiChevronRight,
    mdiChevronUp,
    mdiChevronDown,
    steps: [
      {
        label: "Pick a date",
        image: require("../assets/images/step1.png"),
        image_mobile: require("../assets/images/step1_mobile.png"),
        description:
          "To start, pick a date where you want to create your list. By default, present date is selected once the app is launched.",
        description_mobile: `To start, click the "calendar" button at the top-left corner of the screen to open the calendar and pick a date where you want to create your list. By default, present date is selected once the app is launched.`,
      },
      {
        label: "Adding items",
        image: require("../assets/images/step2.png"),
        image_mobile: require("../assets/images/step2_mobile.png"),
        description:
          "Add items to your list by typing your task in this window.",
        description_mobile: `Add items to your list by clicking the "+" button at the top-right corner of the screen and type your task in window that will pop out.`,
      },
      {
        label: "List navigation",
        image: require("../assets/images/step3.png"),
        image_mobile: require("../assets/images/step3_mobile.png"),
        description: `Items you add will appear on the list of the date you have selected. Clicking the left and right arrow buttons also allows you to change the selected date and go to the previous and next day respectively. By clicking the checkbox beside the items on the list, the item will be marked as done. Lastly, if you wish to delete an item, just click the "X" button at the end of the line of each item.`,
        description_mobile: `Items you add will appear on the list of the date you have selected. Clicking the left and right arrow buttons also allows you to change the selected date and go to the previous and next day respectively. By clicking the checkbox beside the items on the list, the item will be marked as done. Lastly, if you wish to delete an item, just click the "X" button at the end of the line of each item.`,
      },
    ],
  }),
};
</script>
