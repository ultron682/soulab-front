<template>
    <div>
      <form @submit.prevent="handleSubmit">
        <div>
          <label for="firstName">Imię:</label>
          <input v-model="form.firstName" type="text" id="firstName" />
          <span>{{ errors.firstName }}</span>
        </div>
        <div>
          <label for="lastName">Nazwisko:</label>
          <input v-model="form.lastName" type="text" id="lastName" />
          <span>{{ errors.lastName }}</span>
        </div>
        <div>
          <label for="email">Adres e-mail:</label>
          <input v-model="form.email" type="email" id="email" />
          <span>{{ errors.email }}</span>
        </div>
        <div>
          <label for="message">Treść wiadomości:</label>
          <textarea v-model="form.message" id="message"></textarea>
          <span>{{ errors.message }}</span>
        </div>
        <button type="submit">Wyślij</button>
      </form>
      <div v-if="submittedData">
        <h2>Przesłane dane</h2>
        <table>
          <tr><th>Imię</th><td>{{ submittedData.firstName }}</td></tr>
          <tr><th>Nazwisko</th><td>{{ submittedData.lastName }}</td></tr>
          <tr><th>Email</th><td>{{ submittedData.email }}</td></tr>
          <tr><th>Wiadomość</th><td>{{ submittedData.message }}</td></tr>
        </table>
      </div>
    </div>
  </template>
  
  <script>
  import { ref } from 'vue';
  import axios from 'axios';
  import * as yup from 'yup';
  
  export default {
    setup() {
      const form = ref({
        firstName: '',
        lastName: '',
        email: '',
        message: ''
      });
  
      const errors = ref({});
      const submittedData = ref(null);
  
      const validationSchema = yup.object().shape({
        firstName: yup.string().required('Imię jest wymagane'),
        lastName: yup.string().required('Nazwisko jest wymagane'),
        email: yup.string().email('Email jest niepoprawny').required('Email jest wymagany'),
        message: yup.string().required('Treść wiadomości jest wymagana')
      });
  
      const validateForm = async () => {
        try {
          await validationSchema.validate(form.value, { abortEarly: false });
          errors.value = {};
          return true;
        } catch (validationErrors) {
          errors.value = validationErrors.inner.reduce((acc, error) => {
            acc[error.path] = error.message;
            return acc;
          }, {});
          return false;
        }
      };
  
      const handleSubmit = async () => {
        if (await validateForm()) {
          try {
            const response = await axios.post('http://localhost:5555/contact', form.value);
            submittedData.value = response.data;
          } catch (error) {
            console.error('Error submitting form:', error);
          }
        }
      };
  
      return {
        form,
        errors,
        handleSubmit,
        submittedData
      };
    }
  };
  </script>