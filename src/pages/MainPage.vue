<template>
  <div class="wrapper">
    <Memo :memoItems="memoItems"></Memo>
  </div>
</template>

<script>
import Memo from "@/components/Memo/Memo.vue"

export default {
  name: 'MainPage',
  data: () => ({
    memoItems: [
      {
        id: 0,
        name: "Memo1",
        size: {
          width: "250px",
          height: "auto"
        },
        matrix: {
          left: "0px",
          top: "0px",
          zIndex: "0",
        },
        content: "Memo Content 1",
        minimize: false
      },
      {
        id: 1,
        name: "Memo2",
        size: {
          width: "250px",
          height: "auto"
        },
        matrix: {
          left: "750px",
          top: "0px",
          zIndex: "0",
        },
        content: "Memo Content 2",
        minimize: false
      }
    ]
  }),
  components: {
    Memo
  }
}
</script>

<style scoped>
  .wrapper {
    height: inherit;
  }
</style>
