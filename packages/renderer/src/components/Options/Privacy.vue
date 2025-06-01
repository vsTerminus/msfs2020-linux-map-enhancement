<template>

  <div class="card full bg-base-100 shadow-xl ">
    <div class="card-body">
      <h2 class="card-title">Privacy</h2>
      <p>The Windows version would upload logs to a remote server by default. That functionality has been deleted in this fork.</p>
    </div>
  </div>

  <div class="card full bg-base-100 shadow-xl ">
    <div class="form-control">

    </div>
  </div>
</template>

<script>

import { useOptionStore } from "../../stores/optionStore";

export default {
  name: "Privacy",
  setup() {
    const optionStore = useOptionStore();
    return { optionStore };
  },
};
</script>

<style scoped>

</style>