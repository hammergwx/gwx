<template>
  <div id="mask" :class="{active:isShow}">
    <div v-for="(item,index) in cityIndex" :key="index" class="cy">{{item.CityName}}</div>
  </div>
</template>

<script>
import { mapState } from "vuex";
export default {
  props: {
    isShow: {
      type: Boolean
    }
  },
  computed: {
    ...mapState(["cityIndex"])
  },
  created() {}
};
</script>

<style lang="scss" scoped>
#mask {
  width: 80%;
  height: 100%;
  background: #ffffff;
  position: fixed;
  right: 0;
  bottom: 0;
  overflow: hidden;
  overflow-y: auto;
  transform: translateX(100%);
  transition: all 1s ease;
  &.active {
    transform: translateX(0%);
  }
}
.cy {
  width: 100%;
  height: 45px;
  padding: 0 10px;
  line-height: 45px;
  border-bottom: 1px solid #eeeeee;
}
</style>