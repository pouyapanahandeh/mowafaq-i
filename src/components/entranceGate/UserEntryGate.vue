<template>
  <div class="row">
    <div class="col-12 col-md-12 col-sm-12 col-xs-12">
      <q-card flat class=" full-width bg-grey-4">
        <q-card-section>
          <q-input
            outlined
            color="green-7"
            class="full-width q-my-sm"
            label="Enter your email address"
            v-model="adminEmail"
          />
          <q-input
            outlined
            color="green-7"
            v-model="adminPassword"
            :type="isPwd ? 'password' : 'text'"
            label="enter your password"
          >
            <template v-slot:append>
              <q-icon
                :name="isPwd ? 'visibility_off' : 'visibility'"
                class="cursor-pointer"
                @click="isPwd = !isPwd"
              />
            </template>
          </q-input>
        </q-card-section>
        <q-card-actions>
          <q-btn
            class="full-width"
            color="green-7"
            label="Submit"
            v-on:click="adminSignIn()"
          />
        </q-card-actions>
      </q-card>
    </div>
  </div>
</template>
<script>
import { ref } from "vue";
import { useQuasar } from "quasar";
import { db } from "src/boot/firebase.js";
import {
  getAuth,
  onAuthStateChanged,
  signInWithEmailAndPassword
} from "firebase/auth";

export default {
  setup() {
    const $q = useQuasar();
    const adminEmail = ref(null);
    const adminPassword = ref(null);

    return {
      adminEmail: ref(""),
      adminPassword: ref(""),
      isPwd: false
    };
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
    async adminSignIn() {
      if (!this) {
        this.$q.notify({
          color: "red-5",
          textColor: "white",
          icon: "o_warning",
          message: "you must fill the input fields."
        });
      } else {
        const auth = getAuth();
        signInWithEmailAndPassword(auth, this.adminEmail, this.adminPassword)
          .then(userCredential => {
            // Signed in
            const user = userCredential.user;
            this.$q.notify({
              color: "green-6",
              textColor: "white",
              icon: "done",
              message: "you logged in successfully"
            });
            this.$router.push("/dashboard");
          })
          .catch(error => {
            const errorCode = error.code;
            const errorMessage = error.message;
            this.$q.notify({
              color: "red-6",
              textColor: "white",
              icon: "rule",
              message: errorMessage + " " + errorCode
            });
          });
      }
    }
  }
};
</script>
