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
            <q-btn color="green-7" label="check data" @click="dialog = true" />
          </q-item-section>
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
                  <q-tooltip
                    v-if="maximizedToggle"
                    class="bg-white text-primary"
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
                  <q-tooltip
                    v-if="!maximizedToggle"
                    class="bg-white text-primary"
                    >Maximize</q-tooltip
                  >
                </q-btn>
                <q-btn dense flat icon="close" v-close-popup>
                  <q-tooltip class="bg-white text-primary">Close</q-tooltip>
                </q-btn>
              </q-bar>

              <q-card-section>
                <span>Full name:{{ item.fullName }}</span>
                <br />
                <span>{{ item.selectedPlan }}</span>
                <br />
                <span>Age: {{ item.age }}</span>
                <br />
                <span>Email: {{ item.email }}</span>
                <br />
                <span>Residency: {{ item.residency }}</span>
                <br />
                <span>Height: {{ item.heightSize }}</span>
                <br />
                <span>Weight: {{ item.weightSize }}</span>
                <br />
                <span>Wais: {{ item.waistSize }}</span>
                <br />
                <span>Allergies: {{ item.allergies }}</span>
                <br />
                <span>injuries: {{ item.injuries }}</span>
                <br />
                <span
                  >favorite protein source:
                  {{ item.favoriteProteinSource }}</span
                >
                <br />
                <span>Muscle to target: {{ item.muscleToTarget }}</span>
                <br />
                <span>occupation: {{ item.occupationStatus }}</span>
                <br />
                <span>Medicine: {{ item.medHistory }}</span>
                <br />
                <span>{{ item.editor }}</span>
                <br />
                <span>Do you want to use steroid: {{ item.radioAnswer }}</span>
                <br />
                <span
                  >How many days you want to workout:{{
                    item.workoutDate
                  }}</span
                >
                <br />
                <span>When you usually do workout: {{ item.workoutTime }}</span>
                <br />
                <span>Workout goal: {{ item.workoutGoal }}</span>
                <br />
              </q-card-section>
              <q-card-actions>
                <q-btn label="send email" color="green-7" />
              </q-card-actions>
            </q-card>
          </q-dialog>
        </q-item>
      </div>
    </div>
    <div v-else>
      <div class="text-center q-my-xl text-h6">
        <q-icon name="work_outline" />
        No requests yet.
      </div>
    </div>
  </div>
</template>
<script>
import { ref } from "vue";
import { useQuasar } from "quasar";
import { db } from "src/boot/firebase.js";
import { collection, getDocs } from "firebase/firestore";

export default {
  setup() {
    const $q = useQuasar();
    return {
      userPlanRequests: ref([]),
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
      this.userPlanRequests = [];
      querySnapshot.forEach(doc => {
        // doc.data() is never undefined for query doc snapshots
        this.userPlanRequests.unshift(doc.data());
        console.log(this.userPlanRequests);
      });
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
