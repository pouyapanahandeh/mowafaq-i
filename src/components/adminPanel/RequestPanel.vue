<template>
  <div class="q-ma-md">
    <div v-if="requestsExist">
      <span class="text-h6 text-grey-9">
        <q-icon name="work_outline" />
        Requests
      </span>
      <div v-for="item in userPlanRequests" :key="item.userAge">
        <q-item
          class="q-my-sm full-width text-black bg-grey-3"
          style="border: 1px solid black; border-radius: 5px;"
          v-ripple
        >
          <q-item-section>
            {{ item.fullName }}
          </q-item-section>

          <q-item-section>
            <q-item-label>{{ item.email }}</q-item-label>
            <q-item-label caption lines="1">age: {{ item.age }}</q-item-label>
          </q-item-section>

          <q-item-section side>
            <q-btn
              color="green-7"
              label="check data"
              v-on:click="openAccountBaseOnEmail(item.email)"
            />
          </q-item-section>
        </q-item>
      </div>
    </div>
    <div v-else>
      <div class="text-center q-my-xl text-h6">
        <q-icon name="work_outline" />
        No requests yet.
      </div>
    </div>

    <q-dialog
      v-model="dialog"
      persistent
      :maximized="maximizedToggle"
      transition-show="slide-up"
      transition-hide="slide-down"
    >
      <q-card class="bg-grey-2 text-black">
        <q-bar>
          <q-space />

          <q-btn
            dense
            flat
            icon="minimize"
            @click="maximizedToggle = false"
            :disable="!maximizedToggle"
          >
            <q-tooltip v-if="maximizedToggle" class="bg-white text-primary"
              >Minimize</q-tooltip
            >
          </q-btn>
          <q-btn
            dense
            flat
            icon="crop_square"
            @click="maximizedToggle = true"
            :disable="maximizedToggle"
          >
            <q-tooltip v-if="!maximizedToggle" class="bg-white text-primary"
              >Maximize</q-tooltip
            >
          </q-btn>
          <q-btn dense flat icon="close" v-close-popup>
            <q-tooltip class="bg-white text-primary">Close</q-tooltip>
          </q-btn>
        </q-bar>

        <q-card-section class="text-black">
          <div v-for="itm in specificUserPlanRequests" :key="itm.email">
            <span>Full name:{{ itm.fullName }}</span>
            <br />
            <span>Selected Plan: {{ itm.selectedPlan }}</span>
            <br />
            <span>Age: {{ itm.age }}</span>
            <br />
            <span>Email: {{ itm.email }}</span>
            <br />
            <span>Residency: {{ itm.residency }}</span>
            <br />
            <span>Height: {{ itm.heightSize }}</span>
            <br />
            <span>Weight: {{ itm.weightSize }}</span>
            <br />
            <span>Wais: {{ itm.waistSize }}</span>
            <br />
            <span>Allergies: {{ itm.allergies }}</span>
            <br />
            <span>injuries: {{ itm.injuries }}</span>
            <br />
            <span
              >favorite protein source: {{ itm.favoriteProteinSource }}</span
            >
            <br />
            <span>Muscle to target: {{ itm.muscleToTarget }}</span>
            <br />
            <span>occupation: {{ itm.occupationStatus }}</span>
            <br />
            <span>Medicine: {{ itm.medHistory }}</span>
            <br />
            <span>{{ itm.editor }}</span>
            <br />
            <span>Do you want to use steroid: {{ itm.radioAnswer }}</span>
            <br />
            <span>How many days you want to workout:{{ itm.workoutDate }}</span>
            <br />
            <span>When you usually do workout: {{ itm.workoutTime }}</span>
            <br />
            <span>Workout goal: {{ itm.workoutGoal }}</span>
            <br />
          </div>
        </q-card-section>
        <q-card-actions>
          <q-btn label="send email" color="green-7" />
        </q-card-actions>
      </q-card>
    </q-dialog>
  </div>
</template>
<script>
import { ref } from "vue";
import { useQuasar } from "quasar";
import { db } from "src/boot/firebase.js";
import {
  collection,
  getDocs,
  query,
  where,
  onSnapshot
} from "firebase/firestore";

export default {
  setup() {
    const $q = useQuasar();
    return {
      userPlanRequests: ref([]),
      specificUserPlanRequests: ref([]),
      dialog: ref(false),
      maximizedToggle: ref(true)
    };
  },
  created() {
    this.getUserPlanRequests();
  },
  methods: {
    async getUserPlanRequests() {
      const querySnapshot = await getDocs(collection(db, "requests"));
      querySnapshot.forEach(doc => {
        // doc.data() is never undefined for query doc snapshots
        this.userPlanRequests.unshift(doc.data());
      });
    },

    async openAccountBaseOnEmail(email) {
      this.specificUserPlanRequests = [];
      const q = query(collection(db, "requests"), where("email", "==", email));
      const unsubscribe = onSnapshot(q, querySnapshot => {
        querySnapshot.forEach(doc => {
          this.specificUserPlanRequests.unshift(doc.data());
        });
      });
      console.log(this.specificUserPlanRequests);
      this.dialog = true;
    }
  },
  computed: {
    requestsExist: function() {
      if (this.userPlanRequests.length > 0) {
        return true;
      } else {
        return false;
      }
    }
  }
};
</script>
