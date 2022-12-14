<script setup>
import { onMounted, provide, reactive } from "vue";
import { ProviderKey } from "./FormikProvider";

const $emit = defineEmits(["submit"]);

const props = defineProps({
  initialValues: {
    type: Object,
    default: () => ({})
  },
  validate: {
    type: Function,
    default: () => {
      return {};
    }
  }
});

const formData = reactive({
  values: {},
  errors: {},
  handleChange: (name, value) => {
    formData.values[name] = value;
  },
  handleSubmit: async (event) => {
    event.preventDefault();
    formData.isSubmitting = true;
    formData.errors = await props.validate(formData.values);
    if (Object.keys(formData.errors).length > 0) {
      formData.isSubmitting = false;
      return;
    }
    $emit("submit", formData.values);
    formData.isSubmitting = false;
  },
  /**
   * Register a field in the form
   * @param {string} name
   * @param {string, Object} defaultValue
   */
  registerField: (name, defaultValue = "") => {
    formData.values[name] = defaultValue;
  },
  isSubmitting: false
});

provide(ProviderKey, formData);

onMounted(() => {
  Object.keys(props.initialValues).forEach((name) => {
    formData.registerField(name, props.initialValues[name]);
  });
});
</script>

<template>
  <form @submit="formData.handleSubmit">
    <slot
      :values="formData.values"
      :errors="formData.errors"
      :handleSubmit="formData.handleSubmit"
      :isSubmitting="formData.isSubmitting"
    ></slot>
  </form>
</template>
