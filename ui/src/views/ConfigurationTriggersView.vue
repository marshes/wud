<template>
  <v-container fluid>
    <v-row v-for="trigger in triggers" :key="trigger.id">
      <v-col :cols="12" class="pt-2 pb-2">
        <trigger-detail :trigger="trigger" />
      </v-col>
    </v-row>
    <v-row v-if="triggers.length === 0">
      <v-card-subtitle class="text-h6">No triggers configured</v-card-subtitle>
    </v-row>
  </v-container>
</template>

<script>
import TriggerDetail from "@/components/TriggerDetail";
import { getAllTriggers } from "@/services/trigger";

export default {
  data() {
    return {
      triggers: [],
    };
  },
  components: {
    TriggerDetail,
  },

  async beforeRouteEnter(to, from, next) {
    try {
      const triggers = await getAllTriggers();
      next((vm) => (vm.triggers = triggers));
    } catch (e) {
      this.$root.$emit(
        "notify",
        `Error when trying to load the triggers (${e.message})`,
        "error",
      );
    }
    next();
  },
};
</script>
