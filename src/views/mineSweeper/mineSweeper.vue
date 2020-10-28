<template>
  <div class="game">
    <div class="box" v-if="data && data.length">
      <Item
        class="item"
        v-for="(v, i) in data"
        :key="i"
        :data="v"
        :index="i"
        ref="item"
        @click.native="onClick(v, i)"
      ></Item>
    </div>
  </div>
</template>

<script>
import Item from "../../components/mineSweeperItem/mineSweeperItem";
export default {
  name: "mineSweeper",
  components: { Item },
  data() {
    return {
      data: [],
      indexTab: [-10, -9, -8, -1, 0, 1, 8, 9, 10]
    };
  },
  mounted() {
    this.init();
  },
  methods: {
    testArr(arr) {
      console.log("\n\n");
      let temp = "",
        tempA = "";
      arr.forEach((v, i) => {
        temp += v + " ";
        if (i % 9 == 8) {
          console.log(temp + " -> " + (i + 1) / 9);
          temp = "";
        }
      });
      for (let i = 1; i < 10; i++) {
        temp += i + " ";
        tempA += "| ";
      }
      console.log(tempA);
      console.log(temp);
      console.log("\n\n");
    },
    init() {
      this.data.length = 81;
      this.data.fill("0"); // 初始化填充地图
      this.data = this.addMine(this.data); // 埋雷初始化
      this.data = this.addWay(this.data); // 修正路线（根据周边九宫格确定地雷数量）
      this.testArr(this.data); // 打印地图
      this.$forceUpdate(); //全局强制监测并渲染  this.data.splice(1, 0);局部监测 pop push等都ok
    },
    addMine(arr) {
      let n = 9;
      while (n--) {
        let a = this.getNum(this.data);
        arr[a - 1] = "*";
      }
      return arr;
    },
    addWay(arr) {
      arr.forEach((v, i) => {
        if (v == "*") {
          this.indexTab.forEach(n => {
            if (i + n >= 0 && i + n < arr.length && arr[i + n] != "*") {
              //   debugger;
              if (
                (!(i % 9) && (Math.abs(i - n) >= 8 || n < 0)) ||
                (i % 9 == 8 && (Math.abs(i - n) >= 8 || n > 0))
              ) {
                return;
              } else {
                arr[i + n] = (Number(arr[i + n]) + 1).toString();
              }
            }
          });
        }
      });
      return arr;
    },
    getNum(arr) {
      let num = parseInt(Math.random() * 100);
      if (num >= arr.length) return this.getNum(arr);
      return num;
    },
    onClick(v, i) {
      console.log(v, i);
      this.$refs.item[i].onCheck = true;
      //   console.log(this.$refs.item[i]);
    }
  }
};
</script>
<style lang="scss">
@import url("./mineSweeper.scss");
</style>
