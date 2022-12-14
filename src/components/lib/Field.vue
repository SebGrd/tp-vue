<script setup>
import { computed, defineProps, inject, onMounted } from "vue";
import { ProviderKey } from "./FormikProvider";

const { handleChange, registerField, values } = inject(ProviderKey);

const props = defineProps({
  /**
   * Element to render as
   * Can be :
   * - a string (e.g. "input")
   * - a component (e.g. Input)
   * @type { String, Object }
   */
  as: {
    type: [String, Object],
    required: true
  },
  /**
   * Name of the field. Will be used as the key in the form values.
   * @type { string }
   */
  name: {
    type: String,
    required: true
  }
});

onMounted(() => {
  registerField(props.name);
});
const component = computed(() => {
  return props.as;
});
</script>

<template>
  <component
    :is="component"
    v-bind="$attrs"
    :name="name"
    @input="(event) => handleChange(name, event.target.value)"
    :value="values[name]">
    <slot></slot>
  </component>
</template>
