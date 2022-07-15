<style scoped>
.section-bg {
  background-color: rgba(250, 250, 250, 1);
}
</style>

<template>
  <v-app class="section-bg">
    <v-app-bar app color="white" dark>
      <v-container>
        <div class="d-flex align-center">
          <v-img
            alt="Vuetify Logo"
            class="shrink mr-2"
            contain
            src="./assets/logo.svg"
            transition="scale-transition"
          />
        </div>
      </v-container>
    </v-app-bar>

    <v-container>
      <v-main class="mt-6">
        <UserList />
      </v-main>
    </v-container>
  </v-app>
</template>

<script>
import UserList from "./components/UserList";

export default {
  name: "App",
  components: {
    UserList,
  },
};
</script>
