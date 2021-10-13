<template>
  <div class="memo-list" :style="mStyleList">
    <div
        class="memo-list--item"
        v-for="memoItem in memoItems"
        :key="memoItem.id"
        :class="mClassMemo(memoItem.id)"
        @click="handleFocus(memoItem.id)"
        @mouseover="handleHover(memoItem.id)"
        @mouseleave="handleHover(-1)"
    >
      {{ memoItem.name }}
    </div>
  </div>
</template>

<script>
export default {
  name: "MemoList.vue",
  props: {
    width: String,
    memoItems: Array
  },
  computed: {
    mStyleList () {
      return {
        width: this.width
      }
    }
  },
  methods: {
    mClassMemo (id) {
      const parentData = this.$parent.$data;
      const focus = parentData.focusId === id;
      const hover = parentData.hoverId === id;
      return { focus, hover }
    },
    handleFocus(id) {
      this.$emit("onFocus", id)
    },
    handleHover(id) {
      this.$emit("onHover", id)
    }
  }
}
</script>

<style scoped>
  .memo-list {
    height: 100%;
    border-right: 1px solid gray;
    background-color: lightgray;
  }
  .memo-list--item {
    padding: 5px;
    border-bottom: 1px solid darkgray;
    transition: background-color .1s ease-in-out;
  }
  .memo-list--item.hover {
    background-color: cornflowerblue;
    transition: background-color .1s ease-in-out;
  }
  .memo-list--item.focus {
    background-color: dodgerblue;
    transition: background-color .1s ease-in-out;
  }
</style>