<script setup></script>

<template>
  <div v-if="state.isAuthenticated" class="container mt-3">
    <h1>Vue Cookie Auth</h1>
    <br>
    <p> <b>CSRF Token:</b> <br> {{ this.state.csrf }} </p>
    <p>You are logged in!</p>
    <button class="btn btn-primary mr-2" @click="whoami">Who Am I?</button>
    <button class="btn btn-danger" @click="logout">Log out</button>
  </div>
  <div v-else class="container mt-3">
    <h1>Vue Cookie Auth</h1>
    <br />
    <p> <b>CSRF Token:</b> <br> {{ this.state.csrf }} </p>
    <br />
    <h2>Login</h2>
    <form @submit.prevent="this.login">
      <div class="form-group">
        <label for="username">Username</label>
        <input type="text" class="form-control" id="username" name="username" v-model="this.state.username" />
      </div>
      <div class="form-group">
        <label for="username">Password</label>
        <input type="password" class="form-control" id="password" name="password" v-model="this.state.password" />
        <div v-if="this.state.error">
          <small class="text-danger">
            {{this.state.error}}
          </small>
        </div>
      </div>
      <button type="submit" class="btn btn-primary">Login</button>
    </form>
  </div>
</template>

<script>
  export default {
    data() {
      return {
        state : {
          csrf: "",
          username: "",
          password: "",
          error: "",
          isAuthenticated: false,
        }
      }
    },
    methods: {
      getCSRF() {
        fetch("http://localhost:8000/api/csrf/", {
          credentials: "include",
        })
        .then((res) => {
          const csrfToken = res.headers.get("X-CSRFToken");
          this.state.csrf = csrfToken;
          console.log(csrfToken);
        })
        .catch((err) => {
          console.log(err);
        });
      },

      getSession() {
        fetch("http://localhost:8000/api/session/", {
          credentials: "include",
        })
        .then((res) => res.json())
        .then((data) => {
          console.log(data);
          if (data.isAuthenticated) {
            this.state.isAuthenticated = true;
          } else {
            this.state.isAuthenticated = false;
            this.getCSRF();
          }
        })
        .catch((err) => {
          console.log(err);
        });
      },

      whoami() {
        fetch("http://localhost:8000/api/whoami/", {
          headers: {
            "Content-Type": "application/json",
          },
          credentials: "include",
        })
        .then((res) => res.json())
        .then((data) => {
          alert("You are logged in as: " + data.username);
        })
        .catch((err) => {
          console.log(err);
        });
      },

      isResponseOk(response) {
        if (response.status >= 200 && response.status <= 299) {
          return response.json();
        } else {
          throw Error(response.statusText);
        }
      },

      login() {
        fetch("http://localhost:8000/api/login/", {
          method: "POST",
          headers: {
            "Content-Type": "application/json",
            "X-CSRFToken": this.state.csrf,
          },
          credentials: "include",
          body: JSON.stringify(
            {
              username: this.state.username,
              password: this.state.password
            }
          ),
        })
        .then(this.isResponseOk)
        .then((data) => {
          console.log(data);
          this.state.isAuthenticated = true;
          this.state.username = "";
          this.state.password = "";
          this.state.error = "";
        })
        .catch((err) => {
          console.log(err);
          this.state.error = "Wrong username or password.";
        });
      },

      logout() {
        fetch("http://localhost:8000/api/logout", {
          credentials: "include",
        })
        .then(this.isResponseOk)
        .then((data) => {
          console.log(data);
          this.state.isAuthenticated = false;
          this.getCSRF();
        })
        .catch((err) => {
          console.log(err);
        });
      },
    },
    mounted () {
      this.getSession();
    },
  }
</script>

<style scoped></style>
