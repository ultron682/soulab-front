<template>
  <div>
    <h2>Przesłane dane</h2>
    <table>
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
  </div>
</template>

<script>
import { useRoute } from 'vue-router';

export default {
  setup() {
    const route = useRoute();

    const firstName = route.query.firstName;
    const lastName = route.query.lastName;
    const email = route.query.email;
    const message = route.query.message;

    return { firstName, lastName, email, message };
  },
};
</script>