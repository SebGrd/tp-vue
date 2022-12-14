<script setup>
import Formik from "./lib/Formik.vue";
import Field from "./lib/Field.vue";
import ImageSelector from "./lib/ImageSelector.vue";
import { ref } from "vue";

const submitted = ref(false);

const sendForm = (formValues) => {
  console.log("Form has been successfully sent", formValues);
  submitted.value = true;
};

const fakeApiCall = async () => {
  return new Promise((resolve) => {
    setTimeout(() => {
      resolve({});
    }, 1000);
  });
};

const validate = async (values) => {
  const errors = {};
  if (!values.username) {
    errors.username = "Username is required";
  }
  if (!values.password) {
    errors.password = "Password is required";
  }
  if (!values.captcha) {
    errors.captcha = "Captcha is required";
  }
  if (!values.class) {
    errors.class = "Class is required";
  }
  await fakeApiCall();
  return errors;
};

</script>
<template>
  <h1 v-if="submitted" style="color: green">Submitted!</h1>
  <Formik
    v-else
    class="form"
    v-slot="{
      values,
      errors,
      handleSubmit,
      isSubmitting,
    }"
    :initial-values="{ username: 'John' }"
    :validate="validate"
    @submit="sendForm">
    <p>IsSubmitting: {{ isSubmitting }}</p>
    <Field as="input" name="username" placeholder="username..." />
    <Field as="input" name="password" type="password" placeholder="password..." />
    <Field as="select" name="class">
      <option value="1">5IW1</option>
      <option value="2">5IW2</option>
      <option value="3">5IW3</option>
    </Field>
    <Field :as="ImageSelector" name="captcha" />
    <button @click="handleSubmit" :disabled="isSubmitting">Submit</button>
    <ul v-if="errors" class="form__errors">
      <li v-for="error in errors">{{ error }}</li>
    </ul>
  </Formik>
</template>

<style scoped lang="scss">
.form {
  > * {
    display: block;
    margin: 1rem 0;
  }
  &__errors {
    color: red;
  }
}
</style>