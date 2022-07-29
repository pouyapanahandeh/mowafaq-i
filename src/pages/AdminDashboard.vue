<template>
  <RequestPanel />
</template>
<script>
import { useQuasar } from "quasar";
import { db } from "src/boot/firebase.js";
import RequestPanel from "src/components/adminPanel/RequestPanel.vue";
export default {
  components: {
    RequestPanel
  },
  setup() {}
};
</script>
