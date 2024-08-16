<template>
  <div class="data-container">
    <h2>Przesłane dane</h2>
    <table class="data-table">
      <tr>
        <th>Imię</th>
        <td>{{ firstName }}</td>
      </tr>
      <tr>
        <th>Nazwisko</th>
        <td>{{ lastName }}</td>
      </tr>
      <tr>
        <th>Email</th>
        <td>{{ email }}</td>
      </tr>
      <tr>
        <th>Wiadomość</th>
        <td>{{ message }}</td>
      </tr>
    </table>
    <button @click="goHome" class="home-button">Powrót na stronę główną</button>
  </div>
</template>

<script>
import { useRoute } from "vue-router";
import { useRouter } from "vue-router";

export default {
  setup() {
    const route = useRoute();
    const router = useRouter();

    if (
      route.query.firstName === undefined ||
      route.query.lastName === undefined ||
      route.query.email === undefined ||
      route.query.message === undefined
    ) {
      router.push({
        name: "home",
      });
    }

    const goHome = () => {
      router.push({ name: "home" });
    };

    const firstName = route.query.firstName;
    const lastName = route.query.lastName;
    const email = route.query.email;
    const message = route.query.message;

    return { firstName, lastName, email, message, goHome };
  },
};
</script>

<style scoped>
.data-container {
  max-width: 600px;
  margin: 2rem auto;
  padding: 1.5rem;
  border: 1px solid #ccc;
  border-radius: 0.5rem;
  background-color: #f9f9f9;
}

h2 {
  text-align: center;
  margin-bottom: 1.5rem;
  color: #333;
}

.data-table {
  width: 100%;
  border-collapse: collapse;
  margin-top: 1rem;
}

.data-table th,
.data-table td {
  padding: 0.75rem;
  text-align: left;
  border: 1px solid #ddd;
}

.data-table th {
  background-color: #f2f2f2;
  color: #333;
  width: 30%;
}

.data-table td {
  background-color: #fff;
}

.home-button {
  display: block;
  margin: 2rem auto 0;
  padding: 0.75rem 1.5rem;
  font-size: 1rem;
  color: white;
  background-color: #007bff;
  border: none;
  border-radius: 0.25rem;
  cursor: pointer;
  transition: background-color 0.3s ease;
}

.home-button:hover {
  background-color: #0056b3;
}
</style>
