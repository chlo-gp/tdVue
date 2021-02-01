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
    <input
      v-model="searchUsers"
      type="text"
      placeholder="Rechercher un user"
    >
    <svg
      id="sort-users"
      xmlns="http://www.w3.org/2000/svg"
      fill="none"
      viewBox="0 0 24 24"
      stroke="currentColor"
      @click="sortLowestUsers"
    >
      <path
        stroke-linecap="round"
        stroke-linejoin="round"
        stroke-width="2"
        d="M3 4h13M3 8h9m-9 4h6m4 0l4-4m0 0l4 4m-4-4v12"
      />
    </svg>
    <svg
      id="sort-users"
      xmlns="http://www.w3.org/2000/svg"
      fill="none"
      viewBox="0 0 24 24"
      stroke="currentColor"
      @click="sortHighestUsers"
    >
      <path
        stroke-linecap="round"
        stroke-linejoin="round"
        stroke-width="2"
        d="M3 4h13M3 8h9m-9 4h9m5-4v12m0 0l-4-4m4 4l4-4"
      />
    </svg>
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
          <th> Age</th>
        </tr>
      </thead>
      <tbody
        id="tbody-users"
      >
        <tr
          v-for="user in searchedUsers"
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
  data() {
    return {
      results: [],
      errored: false,
      genderFilter: ['male', 'female'],
      searchUsers: ''
    }
  },
  computed: {
    usersFiltered() {
      return this.results.filter(user => this.genderFilter.includes(user.gender))
    },
    searchedUsers() {
      return this.usersFiltered.filter((user) => {
        return (user.name.first + ' ' + user.name.last).match(new RegExp(this.searchUsers, 'i'));

      })
    }
  },
  methods: {
    fetchUsers() {
      axios
          .get('https://randomuser.me/api/?results=20')
          .then(response => (this.results.push.apply(this.results, response.data.results)))

          .catch(error => {
            console.log(error)
            this.errored = true
          })
    },
    sortLowestUsers() {
      this.results.sort((a, b) => a.dob.age > b.dob.age ? 1 : -1);
    },
    sortHighestUsers() {
      this.results.sort((a, b) => a.dob.age < b.dob.age ? 1 : -1);
    }
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

.table {
  padding-top: 30px;
}

th, td {
  padding: 0px 20px;
}
#sort-users {
  width: 40px;
  vertical-align: middle;
  padding: 0 10px;
}
</style>
