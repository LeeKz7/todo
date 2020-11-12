<template>
  <div class="home">
    <div class="from">
      <input type="text" v-model="title" placeholder="标题" />
      <textarea cols="30" rows="10" v-model="content" placeholder="内容"></textarea>
      <button class="btn" @click="add">添加</button>
    </div>
    <div class="list">
      <div class="box" v-for="(item, i) in list" :key="i">
        <div class="box-title">{{ item.title }}</div>
        <div class="box-content">{{ item.content }}</div>
        <div class="box-operation">
          <div>{{ toTime(item.time) }}</div>
          <div @click="over(i, item.state)">{{ item.state ? '已完成' : '完成' }}</div>
          <div @click="del(i)">删除</div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'Home',
  data() {
    return {
      title: '',
      content: '',
      list: []
    }
  },
  created() {
    this.getList()
  },
  methods: {
    add() {
      if (!(this.title && this.content)) {
        return alert('请填写正确!')
      }
      const todo = JSON.parse(window.localStorage.getItem('todo')) || []
      todo.push({
        title: this.title,
        content: this.content,
        time: new Date().getTime(),
        state: false
      })
      window.localStorage.setItem('todo', JSON.stringify(todo))
      this.title = ''
      this.content = ''
      this.getList()
    },
    getList() {
      const todo = JSON.parse(window.localStorage.getItem('todo')) || []
      const newtodo = todo.sort(function(a, b) {
        return a.time - b.time
      })
      this.list = newtodo
    },
    over(i, state) {
      if (!state) {
        this.list[i].state = true
        window.localStorage.setItem('todo', JSON.stringify(this.list))
        this.getList()
      }
    },
    del(i) {
      this.list.splice(i, 1)
      window.localStorage.setItem('todo', JSON.stringify(this.list))
      this.getList()
    }
  },
  computed: {
    toTime() {
      return function(timestamp) {
        const date = new Date(timestamp)
        const Y = date.getFullYear() + '-'
        const M = (date.getMonth() + 1 < 10 ? '0' + (date.getMonth() + 1) : date.getMonth() + 1) + '-'
        const D = date.getDate() + ' '
        const h = date.getHours() + ':'
        const m = date.getMinutes()
        return Y + M + D + h + m
      }
    }
  }
}
</script>

<style scoped>
.from {
  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: column;
}
input {
  width: 200px;
  height: 30px;
  /* border: 1px solid rgb(80, 210, 250); */
  border-radius: 5px;
}
textarea {
  margin-top: 20px;
  width: 200px;
  height: 50px;
  border-radius: 5px;
}
.btn {
  width: 90px;
  margin: 20px;
  height: 50px;
  background: rgb(80, 210, 250);
  border-radius: 10px;
}
.home {
  padding: 50px;
  box-sizing: border-box;
  width: 100%;
  height: 100%;
  background: rgb(243, 243, 243) url(http://imgs.bootstrapmb.com/content/images/bg-page-section.png);
  display: flex;
  align-items: center;
  flex-direction: column;
}
.list {
  display: flex;
  flex-direction: row;
}
.box {
  width: 350px;
  height: 200px;
  padding: 10px;
}
.box-title {
  width: 350px;
  height: 50px;
  background: rgb(80, 210, 250);
  border-top-left-radius: 10px;
  border-top-right-radius: 10px;
  display: flex;
  justify-content: center;
  align-items: center;
}
.box-content {
  width: 350px;
  height: 150px;
  background: white;
  display: flex;
  justify-content: center;
  align-items: center;
}
.box-operation {
  width: 350px;
  height: 50px;
  background: rgb(80, 210, 250);
  display: flex;
  justify-content: center;
  align-items: center;
  border-bottom-left-radius: 10px;
  border-bottom-right-radius: 10px;
}
.box-operation > div {
  width: 33%;
  display: flex;
  justify-content: center;
  align-items: center;
}
</style>
