<template>
  <div class="home">
    <hr />
    {{ time }}
    <p>{{ msg }}</p>
    <p>{{ count }}</p>
    <p>{{ count2.count }}</p>
    <button @click="addCount">增加数量</button>
    <button @click="addCount2">增加数量2</button>
    <button @click="reloadPage">刷新</button>
  </div>
</template>

<script setup>
// @ is an alias to /src
import { ref, reactive, onMounted, onUnmounted, inject } from "vue";
const msg = "hello world";
let count = ref(0);
let time = ref(0);
let timer = ref(null);
let count2 = reactive({
  count: 0,
});
console.log(count2);
function addCount() {
  count.value++;
}
function addCount2() {
  count2.count++;
  // count2.count.value++;
}
const reload1 = inject("reload");
function reloadPage() {
  reload1();
}
function getTime() {
  const time1 = new Date();
  const year = time1.getFullYear();
  const month = time1.getMonth() + 1;
  const day = time1.getDate();
  const hour = time1.getHours();
  const min = time1.getMinutes();
  const sec = time1.getSeconds();
  time.value =
    year + "-" + month + "-" + day + " " + hour + ":" + min + ":" + sec;
}
onMounted(() => {
  console.log("1111111代表又刷新一次");
  getTime();
  timer.value = setInterval(() => {
    getTime();
  }, 1000);
});
onUnmounted(() => {
  clearInterval(timer.value);
  timer.value = null;
});
</script>
