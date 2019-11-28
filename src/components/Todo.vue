<template>
  <div class="todo">
    <div class="jumbotron text-center">
      <h1>Todo task list</h1>
      <p>Creates a new task by typing in an input field and pressing Enter.</p>
      <div class="container mt-5">
        <div class="row">
          <div class="col-sm-4">
            <input name="todo" v-model="task" type="text" class="form-control" placeholder="Enter a new task..." v-on:keyup.enter="saveTodo">
          </div>
        </div>
        <div class="row">
          <div class="col-sm-4 mt-5">
            <ul v-if="todos.length">
              <li class="text-left" v-for="todo of todos"> {{todo.name}} </li>
            </ul>
            <ul v-else>
              <li class="text-left text-danger">No task found.</li>
            </ul>  
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'Todo',
  data () {
    return {
      todos: [],
      task: null
    }
  },
  methods: {
    getTodos: function(){
      this.axios.get(process.env.BASE_URL+"todos").then((response) => {
        this.todos = response.data.data
      }).catch(error => alert(error.message));
    },
    saveTodo: function(event) {
      let todo = {name:this.task}
      if(this.task !== ''){
        this.axios.post(process.env.BASE_URL+"todos",todo).then((response) => {
          console.log(response.data.data)
          this.todos.unshift(response.data.data)
          this.task = null
        }).catch(error => alert(error.message));  
      }
      
    }
  },
  created: function() {
    this.getTodos()
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
</style>
