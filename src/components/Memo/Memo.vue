<template>
  <div
      class="memo-wrapper">
    <MemoList
        :width="'250px'"
        :memoItems="memoItems"
        @onFocus="onFocus"
        @onHover="onHover"
    ></MemoList>
    <div
        class="memo-item-wrapper"
        @mousedown="onMouseEvent"
        @mousemove="onMouseEvent"
        @mouseup="onMouseEvent"
    >
      <div
          class="memo-item-background"
          @mouseover="onHover(-1)"
      ></div>
      <MemoItem
          v-for="memoItem in memoItems"
          v-bind="memoItem"
          :key="memoItem.id"
          @onHover="onHover"
          @onDragStart="onDragStart"
          @onMinimize="onMinimize"
          @onClose="onClose"
      >{{ memoItem.content }}</MemoItem>
    </div>
  </div>
</template>

<script>
import MemoItem from "@/components/Memo/MemoItem";
import MemoList from "@/components/Memo/MemoList";

export default {
  name: "Memo",
  props: {
    memoItems: Array
  },
  data: () => ({
    mouse: {
      addX: 0,
      addY: 0,
      x: 0,
      y: 0,
    },
    focusId: -1,
    hoverId: -1,
    zIndex: 1,
    dragging: false,
  }),
  methods: {
    getItem (id) {
      const focusIndex = this.memoItems.findIndex((item) => item.id === id)
      if (this.memoItems[focusIndex] !== undefined) {
        return [focusIndex, this.memoItems[focusIndex]]
      }
    },
    onHover (id) {
      this.hoverId = id;
    },
    onFocus (id) {
      this.focusId = id;

      const [index, item] = this.getItem(id)
      if (item.matrix.zIndex !== this.zIndex) {
        this.$set(this.memoItems, index, {
          ...item,
          matrix: {
            ...item.matrix,
            zIndex: (this.zIndex++).toString()
          }
        });
      }
    },
    onMouseEvent (event) {
      const mouse = {}
      if (event.type === "mouseup") {
        this.mouse.addX = 0
        this.mouse.addY = 0
        this.focusId = -1
        this.dragging = false;
      }
      mouse.x = event.clientX - this.mouse.addX
      mouse.y = event.clientY - this.mouse.addY
      this.mouse = Object.assign(this.mouse, mouse)

      if (this.dragging && this.focusId > -1) {
        const [index, item] = this.getItem(this.focusId)
        this.$set(this.memoItems, index, {
          ...item,
          matrix: {
            ...item.matrix,
            left: `${mouse.x}px`,
            top: `${mouse.y}px`
          }
        });
      }
    },
    onDragStart (id, matrix) {
      this.mouse.addX = matrix.x
      this.mouse.addY = matrix.y
      this.onFocus(id)
      this.dragging = true;
    },
    onMinimize (id, state) {
      console.log('onMinimize', id, state)
    },
    onClose (id) {
      const [index] = this.getItem(id)
      this.memoItems.splice(index, 1)
    },
  },
  components: {
    MemoList,
    MemoItem
  },
}
</script>

<style scoped>
  .memo-wrapper {
    display: flex;
    width: 100%;
    height: 100%;
  }
  .memo-wrapper > .memo-item-wrapper {
    position: relative;
    width: 100%;
    height: 100%;
    overflow: hidden;
  }
  .memo-item-wrapper > .memo-item-background {
    position: absolute;
    width: inherit;
    height: inherit;
  }
</style>