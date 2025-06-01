<template>
  <div class="absolute inset-x-0 bottom-0 space-y-4">
    <div class="stats shadow bg-neutral-focus flex justify-center">
      <div class="stat">
        <div class="stat-title">Total Loaded Image</div>
        <div class="stat-value text-primary">{{ statics.numOfImageLoaded }}</div>
        <div class="stat-desc">{{ lastLoadTimeFormatted }}</div>
        <div class="stat-desc">({{ statics.lastLoadingTime.toFixed(1) }}s)</div>
      </div>
      <div class="stat">
        <div class="stat-title">Loaded Image</div>
        <div class="stat-value text-secondary">
          <img :src="loadedImageUrl" class="w-16 h-16" />
        </div>
        <div class="stat-desc truncate w-32">
          <a target="_blank" :href="statics.lastLoadingImageUrl">{{ statics.lastLoadingImageUrl }}</a>
        </div>
      </div>
      <div class="stat">
        <div class="stat-title">Cache Hit</div>
        <div class="stat-value text-primary">{{ cacheHit }}/{{ cacheRate }}%</div>
      </div>
      <div class="stat">
        <div class="stat-title">Data Usage</div>
        <div class="stat-value text-primary">{{ usedInMB }}MB</div>
      </div>
    </div>
  </div>
</template>

<script>
import got from "got";
import moment from "moment";
import { useOptionStore } from "../../stores/optionStore";
import log from "electron-log";
import { useStatusStore } from "../../stores/statusStore";

let getStaticInfoInterval = null;
let imageRndInterval = null;


export default {
  name: "RuntimeInfo",
  data() {
    let optionStore = useOptionStore();
    return {
      imageRnd: 0,
      statics: {
        numOfImageLoaded: 0,
        lastLoadingImageUrl: 0,
        lastLoadTime: 0,
        lastLoadingTime: 0,
        cacheHit: 0,
        bytesLoaded: 0
      },
      optionStore,
      statusStore: useStatusStore()
    };
  },
  async mounted() {
    getStaticInfoInterval = setInterval(await this.getStaticInfo, 1000, 1000);
    imageRndInterval = setInterval(() => {
      this.imageRnd = moment().unix();
    }, 100, 100);
  },
  async beforeUnmount() {
    log.info("Runtime info:" + JSON.stringify(this.statics));
    log.info("Total run time(s):" + moment.duration((moment.now() - this.statusStore.appStartTime), 'ms').asSeconds());

    clearInterval(getStaticInfoInterval);
    clearInterval(imageRndInterval);
  },
  computed: {
    loadedImageUrl() {
      return "http://localhost:39871/last-image?rnd=" + this.imageRnd;
    },
    lastLoadTimeFormatted() {
      return moment(this.statics.lastLoadTime).calendar();
    },
    usedInMB() {
      return Math.round(this.statics.bytesLoaded / 1024 / 1024);
    },
    cacheHit() {
      return this.statics.cacheHit;
    },
    cacheRate() {
      return (this.cacheHit / this.statics.numOfImageLoaded * 100).toFixed(1);
    }
  },
  methods: {
    async getStaticInfo() {
      this.statics = await got.get("http://localhost:39871/statics", {
        timeout: {
          request: 2 * 1000
        }
      }).json();
    },
    donate() {

    }
  }
};
</script>

<style scoped>

</style>