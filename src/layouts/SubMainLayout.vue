<template>
  <q-layout view="hHh lpR fFf">
    <q-header reveal bordered class="bg-white text-white">
      <q-toolbar>
        <q-toolbar-title class="text-black">
          N7T team panel
        </q-toolbar-title>
        <q-btn label="logout" color="green-7" v-on:click="signout()" />
      </q-toolbar>
    </q-header>

    <q-page-container>
      <router-view />
    </q-page-container>
  </q-layout>
</template>
<script>
import { useQuasar } from "quasar";
import { db } from "src/boot/firebase.js";
import { getAuth, onAuthStateChanged, signOut } from "firebase/auth";

export default {
  setup() {
    const $q = useQuasar();
  },
  created() {
    const auth = getAuth();
    onAuthStateChanged(auth, user => {
      if (user) {
        this.$router.push("/dashboard");
      } else {
        this.$router.push("/");
      }
    });
  },
  methods: {
    async signout() {
      const auth = getAuth();
      signOut(auth)
        .then(() => {
          // Sign-out successful.
          this.$router.push("/dashboard");
        })
        .catch(error => {
          // An error happened.
        });
    }
  }
};
</script>
