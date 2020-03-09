<template>
  <div class="hello">
    <input v-model="search" placeholder="Поиск">
    <hr>
    <input @keyup.enter="addTodo" v-model="newTodo" placeholder="Новая задача">
    <button style="margin-left: 10px" @click="addTodo">Добавить</button>
    <h1>Задачи</h1>
    <div v-for="todo in sortTodos" :key="todo.name">
      <p>
        <button class="remove" @click="deleteTodo(todo)">X</button>
        <span :class="todo.status ? 'line-through' : 'text-bold'" v-html="todo.name"></span> |
        <span>{{todo.time}}</span>
        <button
          style="margin-left: 10px"
          @click="changeStatus(todo)"
        >{{todo.status ? 'Не сделано!' : 'Сделано!'}}</button>
      </p>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      newTodo: "",
      search: "",
      todos: [
        { name: "<strike>Пропылесосить</strike>", status: false },
        { name: "Вынести мусор", status: false },
        { name: "Проверить учеников / Заработать деньги", status: true }
      ]
    };
  },
  computed: {
    sortTodos: function() {
      return this.todos.sort((x, y) =>
        x.status === y.status ? 0 : x.status ? 1 : -1
      ).filter((todo) => {
        return todo.name.toLowerCase().includes(this.search.toLowerCase())
      });
    }
  },
  methods: {
    addTodo() {
      if (this.newTodo.trim() != "") {
        this.todos.unshift({
          name: this.newTodo,
          status: false,
          time: new Date().toLocaleString()
        });
        this.newTodo = "";
      }
    },
    changeStatus(todo) {
      todo.status = !todo.status;
    },
    deleteTodo(todo) {
      let index = this.todos.indexOf(todo);
      this.todos.splice(index, 1);
    }
  }
};
</script>

<style scoped>
.text-red {
  color: red;
}
.text-green {
  color: green;
}

button {
  background-color: #027be3;
  border: none;
  cursor: pointer;
  color: white;
  padding: 5px 12px;
  border-radius: 5px;
}

.remove {
  background-color: red;
  font-weight: bold;
  border-radius: 50%;
  width: 32px;
  height: 32px;
}
.line-through {
  text-decoration: line-through;
}
.text-bold {
  font-weight: bold;
}
</style>
