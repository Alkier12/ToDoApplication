<template>
  <div class="main-box">
    <div class="main-title">
      {{msg}}
    </div>
    <div class="todo-list">
      <input type="text" class="todoInput" placeholder="Новая задача" v-model="newTodo" @keyup.enter="addTodo">
    </div>
    <div class="save-todo-list" v-for="(todo, index) in todosFiltered" :key="todo.id">
      <div class="todo-rename-box">
        <input class="todo-done-checkbox" type="checkbox" v-model="todo.done">
        <div v-if="!todo.editing" v-on:dblclick="todoRename(todo)" v-bind:class="{ done: todo.done }" class="todo-main-name">{{todo.title}}</div>
        <input v-if="todo.editing" @keyup.enter="saveTodo(todo)" class="todo-rename-input" type="text" v-model="todo.title">
      </div>
      <div class="remove-item" v-on:click="removeElement(index)">&times;</div>
    </div>
    <div class="quantity-tasks">
      <div>
        <label><input type="checkbox" :checked="!selectAll" @change="checkAlltodo">Выбрать всё</label>
      </div>
      <div> Всего: {{allTasks}}</div>
    </div>
    <div class="quantity-tasks">
      <div>
        <button :class="{ active: filter == 'all' }" @click="filter = 'all'">All</button>
        <button :class="{ active: filter == 'active' }" @click="filter = 'active'">Active</button>
        <button :class="{ active: filter == 'completed' }" @click="filter = 'completed'">Completed</button>
      </div>
      <div>
        <div class="deleteAll" name="fade">
          <button @click="clearCompleted">Clear Completed</button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'TodoList',
  data(){
    return{
      newTodo:'',
      todoId: '1',
      editing : false,
      todoDone: '0',
      filter: 'all',
      todos:[

      ],
    }
  },
  computed:{
    allTasks(){
      return this.todos.filter(todo => !todo.done).length
    },
    selectAll(){
      return this.allTasks != 0
    },
    todosFiltered() {
      if (this.filter == 'all') {
        return this.todos
      } else if (this.filter == 'active') {
        return this.todos.filter(todo => !todo.done)
      } else if (this.filter == 'completed') {
        return this.todos.filter(todo => todo.done)
      }
      return this.todos
    },
  },
  props: {
    msg: String
  },
  methods:{
    addTodo(){
      this.todos.push({
        todoId: this.todoId,
        title: this.newTodo,
        completed: false,
        editing: this.editing

      })
      this.newTodo = '';
      this.todoId++
    },
    removeElement(index){
      this.todos.splice(index, 1)
    },
    todoRename(todo){
      todo.editing = true
    },
    saveTodo(todo){
     todo.editing = false
    },
    checkAlltodo(){
      this.todos.forEach((todo) => todo.done = event.target.checked)
    },
    clearCompleted() {
      this.todos = this.todos.filter(todo => !todo.done)
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style>
.main-box{
  margin: 0 auto;
  width: 700px;
  min-height: 250px;
  background-color: black;
  display: flex;
  flex-direction: column;
  align-items: center;
  border-radius: 15px;
  padding-bottom: 35px;
}

.main-title{
  width: 300px;
  display: flex;
  justify-content: center;
  margin-top: 25px;
  color: #fff;
  font-family: "Comic Sans MS";
  font-size: 25px;
  border-bottom: 2px solid #fff;
}

input::-webkit-input-placeholder       {text-indent: 0px;   transition: text-indent 0.4s ease;}
input::-moz-placeholder                {text-indent: 0px;   transition: text-indent 0.4s ease;}
input:-moz-placeholder                 {text-indent: 0px;   transition: text-indent 0.4s ease;}
input:-ms-input-placeholder            {text-indent: 0px;   transition: text-indent 0.4s ease;}
input:focus::-webkit-input-placeholder {text-indent: 500px; transition: text-indent 0.4s ease;}
input:focus::-moz-placeholder          {text-indent: 500px; transition: text-indent 0.4s ease;}
input:focus:-moz-placeholder           {text-indent: 500px; transition: text-indent 0.4s ease;}
input:focus:-ms-input-placeholder      {text-indent: 500px; transition: text-indent 0.4s ease;}


.todoInput{
  width: 300px;
  height: 30px;
  margin-top: 30px;
  margin-bottom: 15px;
  border-radius: 5px;
  font-size: 20px;
  font-family: "Comic Sans MS";
  padding: 5px;

}

.save-todo-list{
  width: 90%;
  display: flex;
  justify-content: space-between;
  margin-top: 9px;
  background-color: #fff;
  font-family: "Comic Sans MS";
  font-size: 22px;
  color: black;
  padding-left: 10px;
  border-radius: 2px;
  padding: 5px;
}

.todo-done-checkbox{
  transform: scale(1.7,1.7);
}

.done{
  text-decoration: line-through;
  color: black;
  transform: rotateX(40grad);
}

.remove-item{
  padding-right: 15px;
  cursor: pointer;
  transition: 0.5s all;
}

.remove-item:hover {
  color: red;
  transform: scale(1.2,1.2);
}

.todo-rename-box{
  display: flex;
  align-items: center;
}

.todo-main-name{
  margin-left: 10px;
  padding: 10px;
  transition: 0.5s all;
}

.todo-rename-input{
  width: 90%;
  background-color: #fff;
  font-family: "Comic Sans MS";
  font-size: 20px;
  color: black;
  padding-left: 10px;
  padding: 5px;
}

.quantity-tasks{
  padding-top: 10px;
  margin-top: 22px;
  font-size: 18px;
  font-family: "Comic Sans MS";
  color: #fff;
  width: 90%;
  display: flex;
  justify-content: space-between;
  border-top: 2px solid #fff;
}

.deleteAll{
  color: #fff;
}




</style>
