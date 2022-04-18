<script setup>
// This starter template is using Vue 3 <script setup> SFCs
// Check out https://vuejs.org/api/sfc-script-setup.html#script-setup
// import HelloWorld from './components/HelloWorld.vue'
import { reactive, ref } from "@vue/reactivity";
import { computed } from "@vue/runtime-core";
import AJsonForm from "./components/AJsonForm.vue";
const defaultSchema = {
  properties: {
    name: {
      type: "string",
      minLength: 1,
      description: "The task's name",
    },
    description: {
      title: "Long Description",
      type: "string",
    },
    done: {
      type: "boolean",
    },
    dueDate: {
      type: "string",
      format: "date",
      description: "The task's due date",
    },
    rating: {
      type: "integer",
      maximum: 5,
    },
    recurrence: {
      type: "string",
      enum: ["Never", "Daily", "Weekly", "Monthly"],
    },
    recurrenceInterval: {
      type: "integer",
      description: "Days until recurrence",
    },
  },
};
const defaultUischema = {
  type: "HorizontalLayout",
  elements: [
    {
      type: "VerticalLayout",
      elements: [
        {
          type: "Control",
          scope: "#/properties/name",
        },
        {
          type: "Control",
          scope: "#/properties/description",
          options: {
            multi: true,
          },
        },
        {
          type: "Control",
          scope: "#/properties/done",
        },
      ],
    },
    {
      type: "VerticalLayout",
      elements: [
        {
          type: "Control",
          scope: "#/properties/dueDate",
        },
        {
          type: "Control",
          scope: "#/properties/rating",
        },
        {
          type: "Control",
          scope: "#/properties/recurrence",
        },
        {
          type: "Control",
          scope: "#/properties/recurrenceInterval",
        },
      ],
    },
  ],
};
const inputSchema = ref(JSON.stringify(defaultSchema));
const inputUischema = ref(JSON.stringify(defaultUischema));

const internalSchema = computed(() => JSON.parse(inputSchema.value));
const internalUiSchema = computed(() => JSON.parse(inputUischema.value));
</script>

<template>
  <!-- <img alt="Vue logo" src="./assets/logo.png" /> -->
  <!-- <HelloWorld msg="Hello Vue 3 + TypeScript + Vite" /> -->
  <div class="json-form">
    <div class="inputSchema">
      <textarea v-model="inputSchema" name="name" id="" cols="50" rows="25" />
      <textarea v-model="inputUischema" name="name" id="" cols="50" rows="25" />
    </div>

    <div class="renderer">
      <AJsonForm :schema="internalSchema" :uiSchema="internalUiSchema" />
    </div>
  </div>
</template>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
.json-form {
  display: flex;
  width: 100%;
}
.inputSchema {
  display: flex;
  flex-direction: column;
}
.renderer {
  width: 100%;
}
</style>
