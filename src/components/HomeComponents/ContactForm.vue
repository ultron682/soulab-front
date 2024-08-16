<template>
  <div>
    <form @submit.prevent="handleSubmit">
      <div class="form-group">
        <label for="firstName">Imię:</label>
        <input v-model="form.firstName" type="text" id="firstName" class="form-control" />
        <span class="error-message">{{ errors.firstName }}</span>
      </div>
      <div class="form-group">
        <label for="lastName">Nazwisko:</label>
        <input v-model="form.lastName" type="text" id="lastName" class="form-control" />
        <span class="error-message">{{ errors.lastName }}</span>
      </div>
      <div class="form-group">
        <label for="email">Adres e-mail:</label>
        <input v-model="form.email" type="email" id="email" class="form-control" />
        <span class="error-message">{{ errors.email }}</span>
      </div>
      <div class="form-group">
        <label for="message">Treść wiadomości:</label>
        <textarea v-model="form.message" id="message" class="form-control"></textarea>
        <span class="error-message">{{ errors.message }}</span>
      </div>
      <button type="submit" class="submit-button">Wyślij</button>
    </form>
    <div v-if="isSavedData == false" class="error-container">
      <h2>Dane nie zostały zapisane w bazie danych</h2>
    </div>
  </div>
</template>
  
  <script>
import { ref } from "vue";
import axios from "axios";
import * as yup from "yup";
import { useRouter } from "vue-router";

export default {
  setup() {
    const form = ref({
      firstName: "",
      lastName: "",
      email: "",
      message: "",
    });

    const errors = ref({});
    const router = useRouter();
    const isSavedData = ref(true);

    const validationSchema = yup.object().shape({
      firstName: yup.string().required("Imię jest wymagane"),
      lastName: yup.string().required("Nazwisko jest wymagane"),
      email: yup
        .string()
        .email("Email jest niepoprawny")
        .required("Email jest wymagany"),
      message: yup.string().required("Treść wiadomości jest wymagana"),
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
          const response = await axios.post(
            "http://localhost:5555/api/contact",
            form.value
          );

          if (response.data.id) {
            isSavedData.value = true;
            // if id exist then saved in database
            router.push({
              name: "contact",
              query: {
                firstName: response.data.firstName,
                lastName: response.data.lastName,
                email: response.data.email,
                message: response.data.message,
              },
            });
          } else {
            isSavedData.value = false;
          }
        } catch (error) {
          console.error("Error submitting form:", error);
          isSavedData.value = false;
        }
      }
    };

    return {
      form,
      errors,
      handleSubmit,
      isSavedData
    };
  },
};
</script>

<style scoped>
.form-group {
  margin-bottom: 1.5rem;
}

.form-control {
  width: 100%;
  padding: 0.75rem;
  font-size: 1rem;
  border: 1px solid #ccc;
  border-radius: 0.25rem;
}

.error-message {
  color: red;
  font-size: 0.875rem;
  margin-top: 0.5rem;
  display: block;
}

.submit-button {
  background-color: #007bff;
  color: white;
  padding: 0.75rem 1.5rem;
  font-size: 1rem;
  border: none;
  border-radius: 0.25rem;
  cursor: pointer;
  transition: background-color 0.3s ease;
}

.submit-button:hover {
  background-color: #0056b3;
}

.error-container {
  margin-top: 1.5rem;
  color: red;
  font-weight: bold;
}
</style>