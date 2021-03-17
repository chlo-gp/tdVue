<template>
  <div class="main-content">
    <!-- Header -->
    <div
      class="header pb-5 d-flex align-items-center"
      style="min-height: 400px;"
    >
      <!-- Mask -->
      <span class="mask bg-gradient-default" />
      <!-- Header container -->
      <div class="container-fluid d-flex align-items-center">
        <div class="col-lg-12">
          <h1 class="display-2 text-white">
            Hello {{ user.firstName }} !
          </h1>
          <p class="text-white mb-5">
            Bienvenue sur ton profil ! Ici tu as accès à tes informations et tu peux les modifier.
          </p>
        </div>
      </div>
    </div>
    <!-- Page content -->
    <div class="container-fluid mt--7">
      <div class="">
        <div class="card card-profile shadow">
          <div class="row justify-content-center">
            <div class="col-lg-3 order-lg-2">
              <div class="card-profile-image">
                <img
                  class="rounded-circle"
                  :src="user.avatarUrl"
                >
              </div>
            </div>
          </div>
          <div class="card-body">
            <div class="text-center">
              <h3>
                {{ user.firstName + " " + user.lastName }}
                <span class="font-weight-light">, {{ user.age + " ans" }}</span> <br>
                <i
                  v-if="user.gender === 'male'"
                  class="fas fa-male"
                />
                <i
                  v-if="user.gender === 'female'"
                  class="fas fa-female"
                />
              </h3>
              <hr class="my-4">
            </div>
          </div>
          <div class="text-center">
            <h3>
              My account
            </h3>
          </div>
          <div class="card-body">
            <user-form
              :user="user"
            />
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios'
import UserForm from "@/components/userForm";

export default {
  name: 'User',
  components: {UserForm},
  data() {
    return {
      user: {
        firstName: '',
        lastName: '',
        email: ''
      },
      errored: false
    }
  },
  created() {
    this.fetchUser()
  },
  methods: {
    fetchUser() {
      axios
          .get(`http://localhost:1501/users/${this.$route.params.id}`)
          .then(response => (this.user = response.data))
          .catch(error => {
            console.log(error)
            this.errored = true
          })
    }
  }
}
</script>

<style>
html {
  font-family: sans-serif;
  line-height: 1.15;
  -webkit-text-size-adjust: 100%;
  -ms-text-size-adjust: 100%;
  -ms-overflow-style: scrollbar;
  -webkit-tap-highlight-color: rgba(0, 0, 0, 0);
}

header,
main,
nav,
section {
  display: block;
}

body {
  font-family: Open Sans, sans-serif;
  font-size: 1rem;
  font-weight: 400;
  line-height: 1.5;
  margin: 0;
  text-align: left;
  color: #525f7f;
  background-color: #f8f9fe;
}

[tabindex='-1']:focus {
  outline: 0 !important;
}

hr {
  overflow: visible;
  box-sizing: content-box;
  height: 0;
}

h1,
h3,
h4,
h5,
h6 {
  margin-top: 0;
  margin-bottom: .5rem;
}

p {
  margin-top: 0;
  margin-bottom: 1rem;
}

a {
  text-decoration: none;
  color: #5e72e4;
  background-color: transparent;
  -webkit-text-decoration-skip: objects;
}

a:not([href]):not([tabindex]) {
  text-decoration: none;
  color: inherit;
}

a:not([href]):not([tabindex]):hover,
a:not([href]):not([tabindex]):focus {
  text-decoration: none;
  color: inherit;
}

a:not([href]):not([tabindex]):focus {
  outline: 0;
}

img {
  vertical-align: middle;
  border-style: none;
}

caption {
  padding-top: 1rem;
  padding-bottom: 1rem;
  caption-side: bottom;
  text-align: left;
  color: #8898aa;
}

label {
  display: inline-block;
  margin-bottom: .5rem;
}

button {
  border-radius: 0;
}

input,
button,
textarea {
  font-family: inherit;
  font-size: inherit;
  line-height: inherit;
  margin: 0;
}

button,
input {
  overflow: visible;
}

button {
  text-transform: none;
}

button,
html [type='button'],
[type='reset'],
[type='submit'] {
  -webkit-appearance: button;
}

button::-moz-focus-inner,
[type='button']::-moz-focus-inner,
[type='reset']::-moz-focus-inner,
[type='submit']::-moz-focus-inner {
  padding: 0;
  border-style: none;
}

::-webkit-file-upload-button {
  font: inherit;
  -webkit-appearance: button;
}

[hidden] {
  display: none !important;
}

h1,
h3,
h4,
h5,
h6 {
  font-family: inherit;
  font-weight: 600;
  line-height: 1.5;
  margin-bottom: .5rem;
  color: #32325d;
}

h1 {
  font-size: 1.625rem;
}

h3 {
  font-size: 1.0625rem;
}

h4 {
  font-size: .9375rem;
}

h5 {
  font-size: .8125rem;
}

h6 {
  font-size: .625rem;
}

.display-2 {
  font-size: 2.75rem;
  font-weight: 600;
  line-height: 1.5;
}

hr {
  margin-top: 2rem;
  margin-bottom: 2rem;
  border: 0;
  border-top: 1px solid rgba(0, 0, 0, .1);
}

.container-fluid {
  width: 100%;
  margin-right: auto;
  margin-left: auto;
  padding-right: 15px;
  padding-left: 15px;
}

.row {
  display: flex;
  margin-right: -15px;
  margin-left: -15px;
  flex-wrap: wrap;
}

.col-lg-3,
.col-lg-6 {
  position: relative;
  width: 100%;
  min-height: 1px;
  padding-right: 15px;
  padding-left: 15px;
}


.card {
  position: relative;
  display: flex;
  flex-direction: column;
  min-width: 0;
  word-wrap: break-word;
  border: 1px solid rgba(0, 0, 0, .05);
  border-radius: .375rem;
  background-color: #fff;
  background-clip: border-box;
}

.card > hr {
  margin-right: 0;
  margin-left: 0;
}

.card-body {
  padding: 1.5rem;
  flex: 1 1 auto;
}

.rounded-circle {
  border-radius: 5% !important;
}

.d-flex {
  display: flex !important;
}

.justify-content-center {
  justify-content: center !important;
}

.align-items-center {
  align-items: center !important;
}

.shadow,
.card-profile-image img {
  box-shadow: 0 0 2rem 0 rgba(136, 152, 170, .15) !important;
}

.my-4 {
  margin-top: 1.5rem !important;
  margin-bottom: 1.5rem !important;
}

.mb-5 {
  margin-bottom: 3rem !important;
}

.mt--7 {
  margin-top: -6rem !important;
}

.text-center {
  text-align: center !important;
}

.font-weight-light {
  font-weight: 300 !important;
}

.text-white {
  color: #fff !important;
}

main {
  display: block;
}

main {
  overflow: hidden;
}

.bg-gradient-default {
  background: linear-gradient(87deg, #82b0ff 0, #9c97ff 100%) !important;
}

[class*='shadow'] {
  transition: all .15s ease;
}

.text-white {
  color: #fff !important;
}

a.text-white:hover,
a.text-white:focus {
  color: #e6e6e6 !important;
}

.card-profile-image img {
  max-width: 200px;
  transition: all .15s ease;
  margin-top: -60px;
}

.main-content {
  position: relative;
}

.main-content {
  position: absolute;
  z-index: 1;
  top: 0;
  left: 0;
  width: 100%;
  padding-right: 0 !important;
  padding-left: 0 !important;
  background-color: transparent;
}

@media (min-width: 768px) {
  .main-content .container-fluid {
    padding-right: 39px !important;
    padding-left: 39px !important;
  }
}

.mask {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  transition: all .15s ease;
}

p {
  font-size: 1rem;
  font-weight: 300;
  line-height: 1.7;
}

.header {
  position: relative;
}

.display-2 span {
  font-weight: 300;
  display: block;
}
</style>


