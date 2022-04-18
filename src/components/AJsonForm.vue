<template>
  <json-forms
    :data="data"
    :renderers="renderers"
    :schema="internalSchema"
    :uischema="internalUiSchema"
    @change="onChange"
  />
</template>

<script>
import { JsonForms } from "@jsonforms/vue";
import {
  defaultStyles,
  mergeStyles,
  vanillaRenderers,
} from "@jsonforms/vue-vanilla";
import { computed, defineComponent } from "vue";
// mergeStyles combines all classes from both styles definitions into one
const myStyles = mergeStyles(defaultStyles, { control: { label: "mylabel" } });
const renderers = [
  ...vanillaRenderers,
  // here you can add custom renderers
];

export default defineComponent({
  name: "AJsonForm",
  components: {
    JsonForms,
  },
  props: {
    uiSchema: {
      type: Object,
      default: {},
    },
    schema: {
      type: Object,
      default: {},
    },
  },
  setup(props) {
    const internalSchema = computed(() => props.schema);
    const internalUiSchema = computed(() => props.uiSchema);
    return {
      internalSchema,
      internalUiSchema,
    };
  },
  data() {
    return {
      // freeze renderers for performance gains
      renderers: Object.freeze(renderers),
      data: {
        name: "Send email to Adrian",
        description: "Confirm if you have passed the subject\nHereby ...",
        done: true,
        recurrence: "Daily",
        rating: 3,
      },
    };
  },
  methods: {
    onChange(event) {
      this.data = event.data;
    },
  },
  provide() {
    return {
      styles: myStyles,
    };
  },
});
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
  margin-left: 120px;
  margin-right: 120px;
}
.mylabel {
  color: darkslategrey;
}
.vertical-layout {
  margin-left: 10px;
  margin-right: 10px;
}
.myform {
  width: 640px;
  margin: 0 auto;
}
.text-area {
  min-height: 80px;
}
.horizontal-layout {
  display: flex;
  width: 100%;
  gap: 10px;
  margin-bottom: 10px;
}

.error {
  color: red;
}
</style>
