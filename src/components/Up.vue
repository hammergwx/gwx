<template>
  <div id="up" :class="{active:ismask}">
    <div class="auto">
      <div class="header">自动定位</div>
      <div class="bei">北京</div>
      <div class="heade">省份</div>
      <div
        @click="show(item.CityID)"
        v-for="(item,index) in cityList"
        :key="index"
        class="con"
        :dataId="item.CityID"
      >
        {{item.CityName}}
        <span class="iconfont icon-angle-right"></span>
      </div>
    </div>
    <Left :isShow="isShow" />
  </div>
</template>

<script>
import Left from "./Left";
import { mapMutations, mapActions, mapState } from "vuex";
export default {
  components: {
    Left
  },
  computed: {
    ...mapState(["cityList"])
  },
  props: {
    ismask: {
      type: Boolean
    }
  },
  methods: {
    ...mapMutations(["getId"]),
    ...mapActions(["gt", "cityL"]),

    show(id) {
      console.log(id);
      this.isShow = true;
      this.getId(id);
      this.cityL();
    }
  },
  created() {
    this.gt();
  },
  data() {
    return {
      isShow: false
    };
  }
};
</script>

<style lang="scss" scoped>
#up {
  width: 100%;
  height: 100%;
  background: #ffffff;
  position: absolute;
  left: 0;
  bottom: 0;
  overflow: hidden;
  transform: translateY(100%);
  transition: all 1s ease;
  .auto {
    width: 100%;
    height: 100%;
    display: flex;
    flex-direction: column;
    overflow-y: auto;
  }
  &.active {
    transform: translateY(0%);
  }
}
#up .header {
  width: 100%;
  height: 20px;
  background: #eeeeee;
  color: #666;
  font-size: 12px;
}
.bei {
  width: 100%;
  height: 45px;
  line-height: 45px;
  padding: 0 20px;
}
.heade {
  width: 100%;
  height: 20px;
  background: #eeeeee;
  font-size: 14px;
}
.con {
  width: 100%;
  height: 45px;
  border-bottom: 1px solid #eeeeee;
  line-height: 45px;
  display: flex;
  box-sizing: border-box;
  position: relative;
}
.con .icon-angle-right {
  position: absolute;
  right: 5px;
}
</style>