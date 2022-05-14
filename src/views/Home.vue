<template>
  <div class="navv">
    <span :class="{'navs' : !this.view_all, 'navs-active' : this.view_all}" @click="selectNav(1)">VIEW ALL</span>
    <span :class="{'navs' : !this.completed, 'navs-active' : this.completed}" @click="selectNav(2)">COMPLETED</span>
    <span :class="{'navs' : !this.ongoing, 'navs-active' : this.ongoing}" @click="selectNav(3)">ONGOING</span>
    <span :class="{'navsPlus' : !this.addit, 'navs-activePlus' : this.addit}" @click="selectNav(4)">+</span>
  </div>

  <div v-if="this.view_all">
    <div class="content" v-for="todo in todos" :key="todo">
     
      <!-- content = 250 characters max -->
      <!-- Lorem ipsum dolor sit amet consectetur adipisicing elit. Ab, corporis blanditiis aut quibusdam, a perspiciatis quam id ratione qui quasi adipisci fugiat amet dolore autem reprehenderit quia, asperiores incidunt rem. Lorem ipsum dolor sit amet consect -->
      <Todo :content="todo.title" :id="todo.id" :progress="todo.inProgress" @changedProgress="toggleOngoing" @remmed="removeTodo" @updatedContent="changeContent"/>
    </div>
  </div>
  <div v-if="this.completed">
    <div class="content" v-for="todo in completed_todos" :key="todo">
      <!-- content = 250 characters max -->
      <!-- Lorem ipsum dolor sit amet consectetur adipisicing elit. Ab, corporis blanditiis aut quibusdam, a perspiciatis quam id ratione qui quasi adipisci fugiat amet dolore autem reprehenderit quia, asperiores incidunt rem. Lorem ipsum dolor sit amet consect -->
      <Todo :content="todo.title" :id="todo.id" :progress="todo.inProgress" @changedProgress="toggleOngoing" @remmed="removeTodo" @updatedContent="changeContent"/>
    </div>
  </div>
  <div v-if="this.ongoing">
    <div class="content" v-for="todo in todos" :key="todo">
      <!-- content = 250 characters max -->
      <!-- Lorem ipsum dolor sit amet consectetur adipisicing elit. Ab, corporis blanditiis aut quibusdam, a perspiciatis quam id ratione qui quasi adipisci fugiat amet dolore autem reprehenderit quia, asperiores incidunt rem. Lorem ipsum dolor sit amet consect -->
      <Todo :content="todo.title" :id="todo.id" :progress="todo.inProgress" @changedProgress="toggleOngoing" @remmed="removeTodo" @updatedContent="changeContent"/>
    </div>
  </div>
  <div v-if="addit">
    <NewTodo @addtodo="updateTodos"/>
  </div>
</template>

<script>
import { computed, onMounted, ref } from 'vue'
import Todo from "../components/Todo.vue"
import NewTodo from "../components/NewTodo.vue"
import axios from "axios";

export default {
  name: 'Home',
  components: {
    Todo, NewTodo
  },
   data: () => ({
      backend_url : 'http://127.0.0.1:8000/api/v1/',
      todos: [],
      completed_todos: [],
      ongoing_todos: [],
      view_all: true,
      completed: true,
      ongoing: true,
      num: 0,
  }),
  methods: {
     selectNav(num){
      if(this.view_all && num != 1){
       this.getCompletedTodos();
        this.view_all = false;
      }
      if(this.completed && num != 2){ 
        this.getAllTodos();
        this.completed = false;
      }
      if(this.ongoing && num != 3){
        this.getCompletedTodos();
        this.ongoing = false;
      }
      if(this.addit && num != 4){
        this.addit = false;
      }
      if(num == 1){
        this.view_all = true;
      }
      else if(num == 2){
        this.completed = true;
      }
      else if(num == 3){
        this.ongoing = true;
      }
      else{
        this.addit = true;
      }},

    getAllTodos(){ 
      axios
      .get(this.backend_url + 'todos').then(response => {
          this.todos = response.data.data
      });  
    },

     getCompletedTodos(){ 
      axios
      .get(this.backend_url + 'todos/completed').then(response => {
          this.completed_todos = response.data.data
      });  
    },
     getOngoingTodos(){ 
      axios
      .get(this.backend_url + 'todos/ongoing').then(response => {
          this.ongoing_todos = response.data.data 
      });  
    },
       toggleOngoing(theId){
      todos.value.forEach((todo) => {
        if(todo.id == theId){
          todo.inProgress = !todo.inProgress
        }
      })
    },
     removeTodo(theId){
      let idx = 1
      todos.value = todos.value.filter((todo) => todo.id != theId)
      todos.value.forEach((todo) => {
        todo.id = idx;
        idx++
      })
    },
     updateTodos(stuff){
      todos.value.push({content: stuff, inProgress: true, id: todos.value.length + 1})
    },
     changeContent(theId, newContent){
      todos.value.forEach((todo) => {
        if(todo.id == theId){
          todo.content = newContent
        }
      })
    }
  },
 
  mounted(){
      this.getAllTodos();  
      this.getCompletedTodos();
      this.getOngoingTodos();
  }
}
</script>

<style scoped>
.navv{
  margin-top: 10px;
  text-align: center;
}
.navs{
  display: inline-block;
  margin: 25px;
  font-size: 0.85em;
  color: gray;
  transition: all 0.2s ease;
}
.navs-active{
  display: inline-block;
  margin: 25px;
  font-size: 0.9em;
  color: rgb(40, 40, 40);
  transition: all 0.2s ease;
}
.navs:hover, .navs-active:hover, .navsPlus:hover, .navs-activePlus:hover{
  cursor: pointer;
}
.content{
  text-align: center;
  margin: 20px auto;
}
.navsPlus{
  transform: translateY(10%);
  margin: 25px;
  display: inline-block;
  font-size: 1.5em;
  color: gray;
  transition: all 0.2s ease;
}
.navs-activePlus{
  transform: translateY(10%);
  display: inline-block;
  margin-left: 22.5px;
  margin-right: 22.5px;
  margin-top: 17.5px;
  margin-bottom: 17.5px;
  font-size: 2em;
  color: rgb(40, 40, 40);
  transition: all 0.2s ease;
}
</style>