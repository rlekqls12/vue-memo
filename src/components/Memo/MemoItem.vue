<template>
  <div
      class="memo"
      :class="mClassMemo"
      :style="mStyleWrap"
      @mouseover="handleHover(id)">
    <div
        class="memo-header"
        @mousedown="handleDragStart">
      <div class="memo-header--content">
        <slot name="header">
          <span class="base-header">{{ mName }}</span>
        </slot>
      </div>
      <div class="memo-header--control">
        <button @click="handleMinimize">-</button>
        <button @click="handleClose">X</button>
      </div>
    </div>
    <transition name="fade">
      <div v-show="!mMinimize" class="memo-content" :style="mStyleContent">
        <slot>
          <p class="base-content">Empty Content</p>
        </slot>
      </div>
    </transition>
  </div>
</template>

<script>
export default {
  name: "MemoItem",
  props: {
    id: Number,
    name: String,
    size: {
      Width: String,
      Height: String
    },
    matrix: {
      left: String,
      top: String,
      zIndex: String
    },
    content: String,
    minimize: Boolean
  },
  data () {
    return {
      mName: this.name ?? "Unknown Memo",
      mSize: this.size ?? { width: "250px", height: "auto" },
      mMatrix: this.matrix ?? { left: "0px", top: "0px", zIndex: "0" },
      mMinimize: this.minimize ?? false,
    }
  },
  methods: {
    handleHover () {
      this.$emit("onHover", this.id)
    },
    handleDragStart (event) {
      const matrix = {
        x: event.clientX - parseInt(this.mMatrix.left),
        y: event.clientY - parseInt(this.mMatrix.top)
      }
      this.$emit("onDragStart", this.id, matrix)
    },
    handleMinimize () {
      this.mMinimize = !this.mMinimize
      this.$emit("onMinimize", this.id, this.mMinimize)
    },
    handleClose () {
      this.$emit("onClose", this.id)
    }
  },
  watch: {
    size () {
      this.mSize = this.size
    },
    matrix () {
      this.mMatrix = this.matrix
    }
  },
  computed: {
    mClassMemo () {
      const parentData = this.$parent.$data;
      const focus = parentData.focusId === this.id;
      const hover = parentData.hoverId === this.id;
      return { focus, hover }
    },
    mStyleWrap () {
      return {
        width: this.mSize.width,
        ...this.mMatrix
      }
    },
    mStyleContent () {
      return {
        height: this?.mMinimize ? "0" : this.mSize.height,
      }
    }
  },
}
</script>

<style scoped>
  .memo {
    position: absolute;
    height: auto;
    margin: 1px;
    border: 1px solid black;
  }
  .memo.hover {
    margin: 0;
    border-width: 2px;
    border-color: cornflowerblue;
  }
  .memo.focus {
    margin: 0;
    border-width: 2px;
    border-color: dodgerblue;
  }
  .memo-header {
    display: flex;
    box-sizing: border-box;
    padding: 5px;
    background-color: #99ddaa;
  }
  .memo-header > .memo-header--content {
    flex: auto;
  }
  .memo-header--content > .base-header {
    font-size: 1rem;
    font-weight: bold;
    color: black;
  }
  .memo-header--control > button {
    min-width: 16px;
    margin: 0 2px;
    padding: 3px;
    cursor: pointer;
    border: none;
    border-radius: 4px;
    background-color: lightgray;
  }
  .memo-content {
    max-height: 500px;
    padding: 5px;
    overflow: hidden;
    border-top: 1px solid black;
    background-color: #ddffee;
  }
  /** Fade Animation **/
  .fade-enter-active, .fade-leave-active {
    transition: height .3s linear, max-height .3s linear;
  }
  .fade-enter, .fade-leave-to {
    max-height: 0;
    padding: 0;
    border-top: 0;
  }
  /** Fade Animation **/
  .memo-content > .base-content {
    padding: 3px 0;
    text-align: center;
    font-size: 0.8rem;
    color: gray;
  }
</style>