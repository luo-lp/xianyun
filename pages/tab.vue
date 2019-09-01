<template>
  <div>
    <span
      v-for="(item, index) in ['新闻1', '新闻2', '新闻3']"
      :key="index"
      @click="active(index)"
      :class="{hotYellow:index===newS}"
    >{{ item }}</span>
    <div v-show="newS===0">1111111111</div>
    <div v-show="newS===1">2222222222</div>
    <div v-show="newS===2">3333333333</div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      newS: 0
    };
  },
  methods: {
    active(index) {
        this.newS=index
    }
  }
};
</script>

<style>
    .hotYellow{
        background-color: yellowgreen;
    }
</style>