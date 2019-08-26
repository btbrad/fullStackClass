<template>
  <div>
    <h2>购物车</h2>
    <table>
      <thead>
        <tr>
          <th>勾选</th>
          <th>课程名称</th>
          <th>课程价格</th>
          <th>数量</th>
          <th>价格</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="item in data"
            :key="item.id"
            :class="{selected: item.isSelected}">
          <td>
            <input type="checkbox"
                   v-model="item.isSelected" />
          </td>
          <td>{{item.name}}</td>
          <td>{{item.price}}</td>
          <td><button @click="decrease(item.id)">-</button>{{item.quantity}}<button @click="increase(item.id)">+</button></td>
          <td>{{item.sum}}</td>
        </tr>
      </tbody>
    </table>
    <div>
      <span>{{selected}}/{{total}}</span>
      <span> 总计：{{totalSum}}</span>
    </div>
  </div>
</template>

<script>
export default {
  name: 'Cart',
  props: {
    data: {
      type: Array,
      default: () => []
    }
  },
  computed: {
    total () {
      return this.data.length
    },
    selected () {
      return this.data.filter(item => item.isSelected).length
    },
    totalSum () {
      const selectedList = this.data.filter(item => item.isSelected)
      const result = selectedList.reduce((accumulator, currentValue) => { return accumulator + currentValue.sum }, 0)
      return result
    }
  },
  methods: {
    decrease (id) {
      this.$emit('quantityChange', { type: 'decrease', id })
    },
    increase (id) {
      this.$emit('quantityChange', { type: 'increase', id })
    }
  },
}
</script>

<style scoped>
.selected {
  color: #f40;
}
</style>