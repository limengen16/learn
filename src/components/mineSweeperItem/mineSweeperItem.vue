<template>
  <div
    class="item"
    :class="{
      mine: data == '*' && onCheck ? !isMine : isMine,
      num: data != '*' && onCheck ? !isMine : isMine
    }"
  >
    {{ onCheck ? (data == "0" ? "" : data) : "" }}
  </div>
</template>

<script>
export default {
  name: "mineSweeperItem",
  data() {
    return {
      onCheck: false,
      isMine: false
    };
  },
  props: {
    data: {
      type: String,
      default: ""
    },
    index: {
      type: Number,
      default: 0
    }
  }
};
</script>
<style lang="scss" scoped>
@import url("./mineSweeperItem.scss");
</style>
