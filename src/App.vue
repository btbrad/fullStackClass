<template>
  <div id="app">
    <h1>{{title}}</h1>
    <hr />
    <div>
      <h2>添加课程</h2>
      <label for="name">课程名称</label><input type="text"
             v-model="newCourse.name"
             id="name">
      <br />
      <label for="price">课程价格</label><input type="text"
             v-model="newCourse.price"
             id="price">
      <br />
      <button @click="addNewCourse">添加课程到列表</button>
    </div>
    <hr />
    <div>
      <h2>课程列表</h2>
      <table>
        <thead>
          <tr>
            <th>课程名称</th>
            <th>课程价格</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="item in courseList"
              :key="item.id">
            <td>{{item.name}}</td>
            <td>{{item.price}}</td>
            <button @click="addToCart(item)">添加到购物车</button>
          </tr>
        </tbody>
      </table>
    </div>
    <hr />
    <Cart :data="shoppingList"
          @quantityChange="handleChange" />
  </div>
</template>

<script>
import Cart from './components/Cart'
export default {
  name: 'app',
  components: {
    Cart
  },
  data () {
    return {
      title: '购物车',
      newCourse: {
        name: '',
        price: ''
      },
      courseList: [
        {
          name: 'web全栈开发架构师',
          price: 1000,
          id: 0
        },
        {
          name: 'Python人工智能',
          price: 1000,
          id: 1
        }
      ],
      shoppingList: [

      ]
    }
  },
  methods: {
    addNewCourse () {
      this.courseList.push({ ...this.newCourse, id: this.courseList.length })
      this.newCourse = {
        name: '',
        price: ''
      }
    },
    addToCart (course) {
      const id = course.id
      // 当前购物车是否已存在
      const flag = this.shoppingList.some(item => item.id === id)
      // 已存在
      if (flag) {
        this.shoppingList.forEach(item => {
          if (item.id === id) {
            item.quantity++
            item.sum = item.price * item.quantity
          }
        })
      } else {
        this.shoppingList.push({ ...course, quantity: 1, sum: course.price, isSelected: false })
      }
    },
    handleChange (obj) {
      this.shoppingList.forEach(item => {
        if (item.id === obj.id) {
          switch (obj.type) {
            case 'decrease':
              if (item.quantity > 1) {
                item.quantity--
              } else {
                window.confirm(`确定要删除${item.name}吗？`) ? this.shoppingList = this.shoppingList.filter(item => item.id !== obj.id) : ''
              }
              break;
            case 'increase':
              item.quantity++
              break;

            default:
              break;
          }
          item.sum = item.price * item.quantity
        }
      })
    }
  }
}
</script>

<style>
/* #app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
} */
</style>
