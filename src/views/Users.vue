<template>
  <h1>VueJS example</h1>
  <div class="buttons">
    <button
      class="button"
      @click="resetFilters"
    >
      Réinitialiser
    </button>
    <button
      class="button"
      @click="showModal"
    >
      Ajouter un utilisateur
    </button>
    <modal
      v-show="isModalVisible"
      @close="closeModal"
    />
  </div>
  <hr>
  <div class="headt">
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
        :disabled="genderFilter.length <2 && genderFilter.includes('male')"
      >
      Hommes
    </label>
    <label>
      <input
        v-model="genderFilter"
        type="checkbox"
        value="female"
        :disabled="genderFilter.length <2 && genderFilter.includes('female')"
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

    <p>
      Trier par âge :
    </p>
    <p v-if="sortDirection === ''">
      Par défaut
    </p>
    <p v-if="sortDirection === 'asc'">
      Croissant
    </p>
    <p v-if="sortDirection === 'desc'">
      Décroissant
    </p>
  </div>
  <p v-if="results.length">
    il y a <strong>{{ searchedUsers.length }}</strong> utilisateurs
  </p>
  <p v-else>
    il n'y a <strong>aucun</strong> utilisateur
  </p>
  <table
    v-if="results.length"
    id="tbl-users"
    class="table table-hover"
  >
    <thead>
      <tr>
        <th />
        <th>Nom</th>
        <th>Email</th>
        <th>Genre</th>
        <th>
          <button
            class="btn btn-light"
            @click="changeSort"
          >
            Âge
            <i
              v-if="sortDirection"
              class="fa"
              :class="[ sortDirection === 'asc' ? 'fa-sort-up' : 'fa-sort-down' ]"
            />
          </button>
        </th>
        <th />
      </tr>
    </thead>
    <tbody
      id="tbody-users"
    >
      <tr
        v-for="user in searchedUsers"
        :key="user.email"
      >
        <td>
          <router-link :to="{name: 'User', params: { id: user.id } }">
            <img
              class="img"
              :src="user.avatarUrl"
            >
          </router-link>
        </td>
        <td>{{ user.firstName }} {{ user.lastName }}</td>
        <td>{{ user.email }}</td>
        <td>{{ user.gender }}</td>
        <td>{{ user.age }}</td>
        <td>
          <button
            class="button"
            @click="deleteUser(user.id)"
          >
            Supprimer
          </button>
        </td>
      </tr>
    </tbody>
  </table>
</template>

<script>
import axios from 'axios';
import Modal from "@/views/Modal";

export default {
  name: 'Users',
  components: {Modal},
  data() {
    return {
      results: [],
      errored: false,
      genderFilter: (this.$route.query.gender || "male,female").split(','),
      searchUsers: this.$route.query.search || '',
      sortDirection: this.$route.query.sortDirection || '',
      isModalVisible: false,
    }
  },
  computed: {
    searchedUsers() {
      return this.results
          .filter(user => this.genderFilter.includes(user.gender))
          .filter((user) => {
            return (user.firstName + ' ' + user.lastName).match(new RegExp(this.searchUsers, 'i'))
          })
          .sort((a, b) => {
            if (!this.sortDirection) return 0;
            const modifier = this.sortDirection === 'desc' ? -1 : 1;
            return (a.birthDate - b.birthDate) * modifier;
          });
    }
  },

  watch: {
    genderFilter() {
      this.updateQuery();
    },
    searchUsers() {
      this.updateQuery();
    },
    sortDirection() {
      this.updateQuery();
    }
  },

  created: function(){
    this.fetchUsers()
  },

  methods: {
    updateQuery() {
      const query = {};
      if (this.genderFilter.length < 2) {
        query.gender = this.genderFilter.join('')
      }
      if (this.searchUsers) {
        query.search = this.searchUsers
      }
      if (this.sortDirection) {
        query.sortDirection = this.sortDirection
      }
      this.$router.push({
        query
      })
    },
    resetFilters() {
      this.genderFilter = ['male', 'female'];
      this.searchUsers = '';
      this.sortDirection = '';
    },
    fetchUsers() {
      axios
          .get('http://localhost:1501/users')
          .then(response => (this.results.push.apply(this.results, response.data)))
          .catch(error => {
            console.log(error)
            this.errored = true
          })
    },
    deleteUser (id) {
      axios
          .delete(`http://localhost:1501/users/`+id)
          .then(() => window.location.reload())
          .then(() => alert("Utilisateur supprimé"))
          .catch(error => {
            console.log(error)
            this.errored = true
          })
    },
    changeSort() {
      if (this.sortDirection === '') {
        this.sortDirection = 'asc'
        return this.results
      } else if (this.sortDirection === 'asc') {
        this.sortDirection = 'desc'
      } else if (this.sortDirection === 'desc') {
        this.sortDirection = ''
      }
    },
    showModal() {
      this.isModalVisible = true;
    },
    closeModal() {
      this.isModalVisible = false;
    },
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h1 {
  margin: 60px 0 0;
  color: #42b983;
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
  vertical-align: middle;
}

#fetch-users {
  background: #42b983;
  color: white;
  border: none;
  padding: 5px 10px;
  border-radius: 3px;
  font-size: 15px;
}

.headt {
  display: flex;
  justify-content: space-around;
}

.btn-primary {
  background-color: #41B883 !important;
  border-color: #41B883 !important;
}

.btn-primary:hover {
  background-color: #35495E !important;
}

.img {
  width: 100px;
}

.buttons {
  display: flex;
  justify-content: space-around;
}

.button {
  background: #64dc94;
  color: white;
  padding: 6px 15px;
  border: none;
  border-radius: 5px;
}
</style>
