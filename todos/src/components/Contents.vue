<template>
  <!-- <div id="app"> -->
    <div class="wrapper">
      <div class="content">
        <h1 class="c-ttl-1">Todo List</h1>
        <!-- [/c-ttl-1] -->
        <div class="c-list-item c-list-item--add">
          <input type="text" name="list" id="list" class="c-list-item__txt" v-model="newList">
          <button class="c-list-btn c-list-btn--add" @click="insertTodo">追加</button>
        </div>
        <!-- [/c-list-item] -->
        <!-- [/リスト表示 ここから] -->
        <div class="c-list-item c-list-item--input" v-for="item in todoLists" :key="item.id">
          <input type="hidden">
          <input type="text" class="c-list-item__txt" v-model="item.list">
          <div class="c-list-item__inner">
            <button class="c-list-btn c-list-btn--update" @click="updateTodo(item.id, item.list)">更新</button>
            <button class="c-list-btn c-list-btn--remove" @click="deleteTodo(item.id)">削除</button>
          </div>
        </div>
        <!-- [/c-list-item] -->
        <!-- [/リスト表示 ここまで] -->
      </div>
      <!-- /[content] -->
    </div>
    <!-- /[wrapper] -->
  <!-- </div> -->
</template>

<script>
import axios from "axios";
export default {
  data() {
    return {
      newList: "",
      todoLists: [],
    };
  },
  methods: {
    async getTodo() {
      const resData = await axios.get("http://127.0.0.1:8000/api/list/");
      this.todoLists = resData.data.data;
    },
    //追加機能
    async insertTodo() {
      const sendData = {
        list: this.newList,
      };
      await axios.post("http://127.0.0.1:8000/api/list/", sendData);
      await this.getTodo();
      //入力後にinputのテキストを削除する
      this.newList = '';
    },
    //更新機能
    async updateTodo(id, list) {
      const sendData = {
        list: list,
      };
      await axios.put("http://127.0.0.1:8000/api/list/" + id, sendData);
      await this.getTodo();
    },
    //削除機能
    async deleteTodo(id) {
      await axios.delete("http://127.0.0.1:8000/api/list/" + id);
      await this.getTodo();
    }
  },
  created() {
    this.getTodo();
  },
};
</script>
