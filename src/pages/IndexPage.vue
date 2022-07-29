<template>
  <q-page class="flex flex-center">
    <UserEntryGate />
  </q-page>
</template>

<script>
import { defineComponent } from "vue";
import UserEntryGate from "src/components/entranceGate/UserEntryGate.vue";

export default defineComponent({
  name: "IndexPage",
  components: {
    UserEntryGate
  }
});
</script>
