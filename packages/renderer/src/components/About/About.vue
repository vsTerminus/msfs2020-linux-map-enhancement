<template>
  <div class="hero h-full bg-base-200">
    <div class="hero-content text-center">
      <div class="max-w-3/4">
        <h1 class="text-5xl font-bold">MSFS2020 Map Enhancement</h1>
        <p class="py-6">Original by He Sicong. Linux Fork by vsTerminus.</p>
        <div class="divider"></div>
        <div class="space-x-4">
          <button class="btn btn-primary btn-sm"><a href="https://github.com/vsTerminus/msfs2020-linux-map-enhancement"
            target="_blank">Github Repository</a></button>
        </div>
        <div class="divider content-center"></div>
        <p class="text-2xl font-extrabold">Linux Fork Changes</p>
        <br/>
        <div>
          <ul>
            <li>Remove remote log uploading</li>
            <li>Remove donation links</li>
            <li>Remove automatic updates</li>
            <li>Remove embedded nginx (You need to run your own)</li>
            <li>Remove embedded python (Uses system python now)</li>
            <li>Update images, branding, package name, etc</li>
            <li>Use dummy hosts file (You need to edit your own /etc/hosts)</li>
            <li>etc.</li>
          </ul>
        </div>
        <div class="divider content-center"></div>
        <p>(I don't know if this works on MSFS 2024, it was built for 2020 and I don't own 2024 yet)</p>
      </div>
    </div>
  </div>
</template>
<script>
import { useStatusStore } from "../../stores/statusStore";

export default {
  name: "About",
  data() {
    let statusStore = useStatusStore();
    return {
      statusStore
    };
  },
  computed: {
    releaseVersion() {
      return this.statusStore.updateInfo ? this.statusStore.updateInfo.releaseName : "";
    },
    releaseNotes() {
      return this.statusStore.updateInfo ? this.statusStore.updateInfo.releaseNotes : "";
    }
  }
};
</script>

<style scoped>

</style>
