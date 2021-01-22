<template>
  <div>
    <h1>VueJS example</h1>
    <hr>
    <button
      id="fetch-users"
      class="btn btn-primary joli"
      @click="fetchUsers"
    >
      Fetch users
    </button>

    <label class="joli">
      <input
        v-model="genderFilter"
        type="checkbox"
        value="male"
      >
      Hommes
    </label>
    <label>
      <input
        v-model="genderFilter"
        type="checkbox"
        value="female"
      >
      Femmes
    </label>
    <div id="divHidden">
      {{ genderFilter }}
    </div>
    <table
      id="tbl-users"
      class="table table-hover"
    >
      <thead>
        <tr>
          <th />
          <th>Nom</th>
          <th>Email</th>
          <th>Tel</th>
          <th>Genre</th>
          <th>Age</th>
        </tr>
      </thead>
      <tbody
        id="tbody-users"
      >
        <tr
          v-for="user in usersFiltered"
          :key="user.email"
        >
          <td><img :src="user.picture.thumbnail"></td>
          <td>{{ user.name.first }} {{ user.name.last }}</td>
          <td>{{ user.email }}</td>
          <td>{{ user.phone }}</td>
          <td>{{ user.gender }}</td>
          <td>{{ user.dob.age }}</td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
import axios from 'axios'

export default {
  name: 'Users',
  data () {
    return {
      results: [],
      errored: false,
      genderFilter: ['male', 'female']
    }
  },
  computed: {
    usersFiltered() {
      return this.results.filter(user => this.genderFilter.includes(user.gender))
    }
  },
  methods: {
    fetchUsers() {
      axios
          .get('https://randomuser.me/api/?results=20')
          .then(response => (this.results = response.data.results))
          .catch(error => {
            console.log(error)
            this.errored = true
          })
    },
  },
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h1 {
  margin: 20px 0 0;
  color: red;
}

ul {
  list-style-type: none;
  padding: 0;
}

li {
  display: inline-block;
  margin: 0 10px;
}

a {
  color: #42b983;
}

#divHidden {
  display: none;
}

.joli {
  margin-right: 10px;
}
</style>
