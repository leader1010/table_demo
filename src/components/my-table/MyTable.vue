<template>

  <table class="table table-bordered table-striped">
    <thead>
    <slot name="header"></slot>
    </thead>

    <tbody>
    <tr v-for="(item, index) in data" :key="item.id">
      <slot name="body" :row="item" :index="index"></slot>
    </tr>
    </tbody>
  </table>


</template>

<script>
export default {
  name: "MyTable",
  props: {
    data: {
      type: Array,
      default: [],
      required: true
    }
  }

}


</script>

<style scoped lang="less">


</style>