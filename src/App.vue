<template>
  <my-table :data="goodList">
    <template v-slot:header>
      <th>序号</th>
      <th>商品名称</th>
      <th>价格</th>
      <th>标签</th>
      <th>操作</th>
    </template>

    <template v-slot:body="{row, index}">
      <td>{{ index + 1 }}</td>
      <td>{{ row.goods_name }}</td>
      <td>¥{{ row.goods_price }}</td>
      <td>
        <input type="text" v-if="row.inputVisible" v-focus
               @blur="onInputConfirm(row)" v-model.trim="row.inputValue"
               @keyup.enter="onInputConfirm(row)" @keyup.esc="row.inputValue = '' "
        >
        <button class="btn btn-primary btn-sm" v-else @click="row.inputVisible=true">+tag</button>
        <span class="badge badge-warning ml-2" v-for="item in row.tags" :key="item">{{ item }}</span>
      </td>
      <td>
        <button class="btn btn-danger btn-sm" @click="remove_goods(row.id)">删除</button>
      </td>
    </template>


  </my-table>
</template>

<script>

import MyTable from "./components/my-table/MyTable.vue";


export default {
  name: 'App',
  created() {
    this.getGoodsList()
  },
  data() {
    return {
      goodList: []
    }
  },
  props: {},

  methods: {
    async getGoodsList() {
      const {data: res} = await this.$http.get('/api/goods')
      if (res.status !== 0) return console.log("获取商品列表信息失败")
      this.goodList = res["data"]
    },

    remove_goods(id) {
      this.goodList = this.goodList.filter(good => good.id !== id)
    },

    onInputConfirm(row) {
      const value = row.inputValue
      row.inputValue = ''
      row.inputVisible = false
      if (!value || row.tags.indexOf(value) !== -1) return
      row.tags.push(value)
    }
  },
  // 自定义指令
  directives: {
    focus(el) {
      el.focus()
    }
  },

  components: {
    MyTable,
  }
}
</script>

<style lang="less" scoped>


</style>
