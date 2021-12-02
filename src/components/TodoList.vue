<template>
  <div class = "TodoList">
    <h1 v-html="title"></h1>
    <div id="add">
      <input type="text" v-model="newItem" @keydown.enter="addNewItem" id="newItem">
      <button @click="addNewItem" id="addNewItem">添加</button>
    </div>
    <h3>进行中({{undoneLength}})</h3>
      <ul>
        <li v-for="(item, index) in items" :key="index" v-show="!item.done">
          <!-- 完成复选框 -->
          <input :id="index" type="checkbox" v-model="item.done" @change="save()"/>
          <label :for="index"></label>
          <!-- 待办事项 -->
          <span v-text="item.title" class="itemTitle"></span>
          <!-- 删除按钮 -->
          <button @click="deleteItem(index)" class="btn_del">删除</button>
        </li>
      </ul>
    <h3>已完成({{doneLength}})</h3>
      <ul>
        <li v-for="(item, index) in items" :key="index" v-show="item.done">
          <!-- 完成复选框 -->
          <input :id="index" type="checkbox" v-model="item.done" @change="save()"/>
          <label :for="index"></label>
          <!-- 待办事项 -->
          <span v-text="item.title" class="itemTitle"></span>
          <!-- 删除按钮 -->
          <button @click="deleteItem(index)" class="btn_del">删除</button>
        </li>
      </ul>
  </div>
</template>

<script>
export default {
  name: 'TodoList',
  data () {
    return {
      title: 'TodoList', // 标题
      items: [], // 待办事项列表
      newItem: '' // 输入框的值
    }
  },
  created () {
    // 初始化数据
    let items = localStorage.items
    if (items) {
      this.items = JSON.parse(items)
    }
  },
  computed: {
    undoneLength () { // 计算未完成的元素长度
      let count = 0
      this.items.map(item => {
        if (!item.done) {
          count++
        }
      })
      return count
    },
    doneLength () { // 计算已完成的元素长度
      let count = 0
      this.items.map(item => {
        if (item.done) {
          count++
        }
      })
      return count
    }
  },
  methods: {
    // 点击添加按钮，添加新的事项
    addNewItem () {
      // push items添加事项
      this.items.push({
        title: this.newItem, // todo标题
        done: false // 完成状态
      })
      this.newItem = '' // 清空输入框
      this.save() // 同步数据
    },
    // 点击删除按钮，删除对应事项
    deleteItem (index) {
      this.items.splice(index, 1) // 删除对应事项
      this.save() // 同步数据
    },
    // 同步数据
    save () {
      localStorage.items = JSON.stringify(this.items)
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h1 {
  font-weight: normal;
  font-family: STXingkai;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  margin: 5px;
}
#newItem {
  vertical-align:middle;
  height: 8px;
  padding: 12px 16px;
  font-size: 16px;
  border: solid 2px #abeeed;
  border-radius: 10px;
  outline: none;
}
#addNewItem {
  vertical-align:middle;
  position: relative;
  left: -14px;
  border-radius: 0 10px 10px 0;
  border: none;
  font-size: 16px;
  text-align: center;
  background-color: #abeeed;
  color: black;
  width: 90px;
  height: 36px;
  outline: none;
  cursor: pointer;
  font-family: YouYuan;
}
#addNewItem:hover {
  background-color: #a3f6f5;
}
.btn_del {
  vertical-align:middle;
  position: relative;
  background-color: #abeeed;
  border-radius: 10px;
  border: none;
  font-size: 14px;
  color: black;
  width: 50px;
  height: 26px;
  outline: none;
  cursor: pointer;
  font-family: YouYuan;
}
.btn_del:hover {
  background-color: #a3f6f5;
}
.itemTitle {
  vertical-align:middle;
  display: inline-block;
  vertical-align:middle;
  height: 18px;
  padding: 2px 5px;
  border-radius: 10px;
  border: solid 2px #abeeed;
  min-width: 150px;
}
input[type=checkbox] {
  position: absolute;
  clip: rect(0, 0, 0, 0);
}
input[type="checkbox"] + label::before {
  content: "\a0";  /*不换行空格*/
  display: inline-block;
  vertical-align: .2em;
  width: .8em;
  height: .8em;
  margin-right: .2em;
  border-radius: .2em;
  background-color: #abeeed;
  text-indent: .15em;
  line-height: .65;  /*行高不加单位，子元素将继承数字乘以自身字体尺寸而非父元素行高*/
  cursor: pointer;
}
input[type="checkbox"]:checked + label::before {
  content: "\2713";
  background-color: #abeeed;
  cursor: pointer;
}
</style>