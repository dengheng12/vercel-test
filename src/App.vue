<template>
  {{ time }}--{{ isAlive }}
  <router-view v-if="isAlive" />
</template>
<script>
import { provide, nextTick, ref } from "vue";
export default {
  data() {
    return {
      time: 0,
      timer: null,
    };
  },
  setup() {
    const isAlive = ref(true);
    function reload() {
      isAlive.value = false;
      nextTick(() => {
        isAlive.value = true;
      });
    }

    provide("reload", reload);
    return {
      isAlive,
    };
  },
  methods: {
    getTime() {
      const time = new Date();
      const year = time.getFullYear();
      const month = time.getMonth() + 1;
      const day = time.getDate();
      const hour = time.getHours();
      const min = time.getMinutes();
      const sec = time.getSeconds();
      this.time =
        year + "-" + month + "-" + day + " " + hour + ":" + min + ":" + sec;
    },
  },
  mounted() {
    this.getTime();
    this.timer = setInterval(() => {
      this.getTime();
    }, 1000);
  },
  unmounted() {
    clearInterval(this.timer);
  },
};
</script>

<style lang="scss">
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}
</style>
