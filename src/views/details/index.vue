<template>
  <div class="xiang_page">
    <img class="banimg" :src="list.CoverPhoto" alt @click="tiaoimg(list)" />
    <div class="carmsg">
      <p>
        <b v-if="list.market_attribute">{{list.market_attribute.dealer_price}}</b>
        <span v-if="list.market_attribute">指导价：{{list.market_attribute.official_refer_price}}</span>
      </p>

      <button v-if="list">{{list.BottomEntranceTitle}}</button>
    </div>
    <div class="timemsg">
      <span
        :class="{active:ind===index}"
        v-for="(item,index) in getYear"
        :key="index"
        @click="()=>ind=index"
      >{{item}}</span>
    </div>
    <div class="inhale_type" v-for="(item,index) in listEach" :key="index">
      <div class="title">{{item.exhaust_str}}/{{item.max_power_str}}{{item.inhale_type}}</div>
      <div class="text">
        <span>{{item.market_attribute.year}}款 {{item.car_name}}</span>
        <span>{{item.max_power}}马力/{{item.trans_type}}</span>
        <li v-if="item.market_attribute">
          指导价：
          <span>{{item.market_attribute.dealer_price_max}}</span>
          <b>{{item.market_attribute.dealer_price_min}}</b>
        </li>
        <i
          class="ibtn"
          v-if="list.BottomEntranceTitle"
          @click="xiao(item.car_id,item.car_name)"
        >{{list.BottomEntranceTitle}}</i>
      </div>
    </div>
    <div class="btn" v-if="list.BottomEntranceTitle">
      <b>{{list.BottomEntranceTitle}}</b>
      <span>本地经销商为您报价</span>
    </div>
  </div>
</template>
<script>
export default {
  props: {},
  components: {},
  data() {
    return {
      list: [],
      carlist: [],
      ind: 0,
      gets: [],
      carId: ""
    };
  },
  created() {
    this.$http
      .get(
        `https://baojia.chelun.com/v2-car-getInfoAndListById.html?SerialID=${this.$route.params.id}`
      )
      .then(res => {
        this.list = res.data.data;
        this.carlist = this.list.list;
      });
    this.getCarid();
  },
  mounted() {},
  computed: {
    getYear() {
      let arr = ["全部"];
      let data = JSON.stringify(this.list.list);
      data &&
        JSON.parse(data).map(item => {
          if (arr.find(a => a === item.market_attribute.year)) return;
          arr.push(item.market_attribute.year);
        });
      return arr;
    },
    listEach() {
      let list = JSON.stringify(this.list.list);
      return (
        list &&
        JSON.parse(list).filter(item => {
          if (this.getYear[this.ind] === "全部") return item;
          return this.getYear[this.ind] === item.market_attribute.year;
        })
      );
    }
  },
  methods: {
    tiaoimg(item) {
      this.$router.push({
        path: "/picture",
        query: {
          SerialID: item.SerialID
        }
      });
    },
    xiao(carId, carName) {
      console.log(carId);
      this.$router.push({
        path: "/cart",
        query: {
          carId: carId,
          AliasName: this.list.AliasName,
          Picture: this.list.Picture,
          carName: carName
        }
      });
    },
    tab(ind) {
      this.ind = ind;
    },
    getCarid() {
      let ids = this.carlist.map((item, index) => {
        return item;
        console.log(item);
      });
    }
  }
};
</script>
<style lang="scss" scoped>
.xiang_page {
  width: 100%;
  height: 100%;
  display: flex;
  overflow: hidden;
  overflow-y: auto;
  flex-direction: column;
  background: #eee;
  box-sizing: border-box;
  .banimg {
    width: 100%;
    height: 180px;
    flex-shrink: 0;
  }
  .carmsg {
    width: 100%;
    height: 60px;
    display: flex;
    justify-content: space-around;
    align-items: center;
    background: #fff;
    flex-shrink: 0;
    p {
      display: flex;
      flex-direction: column;

      b {
        font-size: 20px;
        color: #f00;
        margin-bottom: 3px;
      }
      span {
        color: #ccc;
      }
    }
    button {
      width: 145px;
      height: 40px;
      background: #09f;
      border: 0;
      border-radius: 3px;
      outline: none;
      color: #fff;
      font-size: 16px;
    }
  }
  .timemsg {
    width: 100%;
    height: 50px;
    line-height: 50px;
    background: #fff;
    margin-top: 10px;
    flex-shrink: 0;
    span {
      margin: 0 10px;
      &.active {
        color: #09f;
      }
    }
    .inhale_type {
      width: 100%;
      display: flex;
      flex-direction: column;
    }
  }
}
.inhale_type {
  background: #eee;
  flex-shrink: 0;
}
.title {
  margin: 5px 10px;
  width: 100%;
  height: 40px;
  line-height: 40px;
  flex-shrink: 0;
}
.text {
  width: 100%;
  height: 150px;
  display: flex;
  flex-direction: column;
  background: #fff;
  flex-shrink: 0;
  // align-items: center;
  i {
    width: 100%;
    height: 40px;
    line-height: 40px;
    text-align: center;
    border-top: 1px solid #ccc;
    color: #09f;
    font-size: 20px;
  }
  span,
  li {
    margin: 5px 10px;
  }
  span:first-child {
    margin-top: 10px;
    font-size: 15px;
  }
  span:nth-child(2) {
    color: #aaa;
  }
  li {
    text-align: right;
    margin-right: 3px;
    color: #aaa;
    span {
      margin: 0 5px;
    }
    b {
      font-size: 20px;
      color: #f00;
      margin: 0 5px;
    }
  }
}
.btn {
  width: 100%;
  height: 50px;
  display: flex;
  flex-direction: column;
  text-align: center;
  background: #09f;
  color: #fff;
  position: fixed;
  bottom: 0;
  left: 0;
  flex-shrink: 0;
  b {
    font-weight: normal;
    // font-size: 18px;
    // margin: 5px;
  }
}
.ibtn {
  font-style: normal;
}
</style>
