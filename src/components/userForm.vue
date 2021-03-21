<template>
  <form
    @submit.prevent="updateUser"
  >
    <div class="pl-lg-4">
      <div class="row">
        <div class="col-lg-6">
          <div class="form-group focused">
            <label
              class="form-control-label"
              for="firstName"
            >First name</label>
            <input
              id="firstName"
              v-model="updatedUser.firstName"
              type="text"
              class="form-control form-control-alternative"
              placeholder="First name"
            >
          </div>
        </div>
        <div class="col-lg-6">
          <div class="form-group">
            <label
              class="form-control-label"
              for="lastName"
            >Last name</label>
            <input
              id="lastName"
              v-model="updatedUser.lastName"
              type="text"
              class="form-control form-control-alternative"
              placeholder="Last Name"
            >
          </div>
        </div>
      </div>
      <div class="row">
        <div class="col-lg-12">
          <div class="form-group focused">
            <label
              class="form-control-label"
              for="email"
            >Email</label>
            <input
              id="email"
              v-model="updatedUser.email"
              type="email"
              class="form-control form-control-alternative"
              placeholder="Your email"
            >
          </div>
        </div>
      </div>
    </div>
    <input
      type="submit"
      class="btn btn-info"
      value="Modifier le profil"
    >
  </form>
</template>
<script>

import axios from "axios";

export default {
  name: "UserForm",
  props: {
    user: Object
  },
  data() {
    return {
      updatedUser: this.user,
    }
  },
  watch: {
    user() {
      this.updatedUser = {...this.user};
    }
  },
  methods: {
    updateUser() {
      axios.put(`http://localhost:1501/users/${this.user.id}`, this.updatedUser)
          .then(response => {
            //window.location.reload()
            console.log(response.data)
            alert("Utilisateur modifié")
          })
          .catch(err => {
            console.log(err)
            if (err.response.status === 403) {
              alert("Cet email est déjà utilisé, veuillez en saisir un autre")
            }
            this.errored = true
          })

    }
  }
}
</script>

<style scoped>
.form-control {
  font-size: 1rem;
  line-height: 1.5;
  display: block;
  width: 100%;
  height: calc(2.75rem + 2px);
  padding: .625rem .75rem;
  transition: all .2s cubic-bezier(.68, -.55, .265, 1.55);
  color: #8898aa;
  border: 1px solid #cad1d7;
  border-radius: .375rem;
  background-color: #fff;
  background-clip: padding-box;
  box-shadow: none;
}

.form-control:focus {
  color: #8898aa;
  border-color: rgba(50, 151, 211, .25);
  outline: 0;
  background-color: #fff;
  box-shadow: none;
}

.form-control:-ms-input-placeholder {
  opacity: 1;
  color: #adb5bd;
}

.form-control:disabled,
.form-control[readonly] {
  opacity: 1;
  background-color: #e9ecef;
}

.form-group {
  margin-bottom: 1.5rem;
}

.btn {
  font-size: 1rem;
  font-weight: 600;
  line-height: 1.5;
  display: inline-block;
  padding: .625rem 1.25rem;
  user-select: none;
  transition: color .15s ease-in-out, background-color .15s ease-in-out, border-color .15s ease-in-out, box-shadow .15s ease-in-out;
  text-align: center;
  vertical-align: middle;
  white-space: nowrap;
  border: 1px solid transparent;
  border-radius: .375rem;
}


.btn:not(:disabled):not(.disabled) {
  cursor: pointer;
}

.btn:not(:disabled):not(.disabled):active {
  box-shadow: none;
}

.btn:not(:disabled):not(.disabled):active:focus {
  box-shadow: 0 7px 14px rgba(50, 50, 93, .1), 0 3px 6px rgba(0, 0, 0, .08), none;
}

.btn-info {
  color: #fff;
  border-color: #11cdef;
  background-color: #11cdef;
  box-shadow: 0 4px 6px rgba(50, 50, 93, .11), 0 1px 3px rgba(0, 0, 0, .08);
}

.btn-info:hover {
  color: #fff;
  border-color: #11cdef;
  background-color: #11cdef;
}

.btn {
  font-size: .875rem;
  position: relative;
  transition: all .15s ease;
  letter-spacing: .025em;
  text-transform: none;
  will-change: transform;
}

.btn:hover {
  transform: translateY(-2px);
  box-shadow: 0 7px 14px rgba(50, 50, 93, .1), 0 3px 6px rgba(0, 0, 0, .08);
}

.form-control-label {
  font-size: .875rem;
  font-weight: 600;
  color: #525f7f;
}

.form-control {
  font-size: .875rem;
}

.form-control:focus:-ms-input-placeholder {
  color: #adb5bd;
}

.form-control:focus::-ms-input-placeholder {
  color: #adb5bd;
}

.form-control-alternative {
  transition: box-shadow .15s ease;
  border: 0;
  box-shadow: 0 1px 3px rgba(50, 50, 93, .15), 0 1px 0 rgba(0, 0, 0, .02);
}

.form-control-alternative:focus {
  box-shadow: 0 4px 6px rgba(50, 50, 93, .11), 0 1px 3px rgba(0, 0, 0, .08);
}

.focused .form-control {
  border-color: rgba(50, 151, 211, .25);
}

.focused .form-control {
  border-color: rgba(50, 151, 211, .25);
}
</style>