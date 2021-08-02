<template>
  <div>
    <input type="text" placeholder="Ne yapılması gerekiyor?" class="input-todo" v-model="newTodo" @keyup.enter="addTodo">
    <div v-for="(todo, index) in todosFiltered" :key="todo.id" class="item-todo">
      <div class="todo-item-left">
        <input type="checkbox" v-model="todo.completed">
          <div v-if="!todo.editingMode" class="todo-item-label" @dblclick="changeMode(index, todo.title)" :class="{ completed: todo.completed}">
          {{todo.title}}
        </div>
        <input @keyup.enter="changeMode(index,todo.title)" @blur="changeMode(index, todo.title)" v-else type="text" v-model="todo.title" class="todo-item-edit" @keyup.esc="cancelEdit(todo)">
        <div class="remove-button"  @click="removeTodo(index)">
          &times;
        </div>
      </div>

    </div>
  </div>

  <div class="extra-container">
    <div>
      <label>
        <input type="checkbox" :checked="!anyRemaining" @change="checkAllTodos"> Tümünü Seç
      </label>
    </div>
    <div>
      {{ remaining }} yapılacak madde kaldı
    </div>
  </div>

    <div class="extra-container">
        <div>
            <button :class="{active: filter == 'all'}" @click="filter = 'all' " >
                Tümü
            </button>
            <button :class="{active: filter == 'active'}" @click="filter = 'active' " >
                Aktif
            </button>
            <button :class="{active: filter == 'completed'}" @click="filter = 'completed'" >
                Tamamlanmış
            </button>
        </div>
    </div>
</template>

<script>
export default {
  name: 'todo-list',
  data(){
      return{
        newTodo: '',
        idForTodo: 3,
        todos: [
          {
            'id': 1,
            'title': 'Starbucks için hazırlan',
            'completed': false,
            'editingMode': false,

          },
          {
            'id': 2,
            'title': 'Motoru al',
            'completed': false,
            'editingMode': false,

          },
        ],
        editCache :'',
        filter: "all",

      }
  }, 
  directives: {
    focus: {
      inserted: function (el) {
        el.focus()
      }
    }
  },
  computed:{
    remaining(){
      return this.todos.filter(todo => !todo.completed).length
    },
    anyRemaining(){
      return this.remaining != 0
    },
    todosFiltered(){
        if(this.filter == 'all'){
            return this.todos;
        }else if(this.filter == 'active'){
            return this.todos.filter(todo => !todo.completed)
        }else if(this.filter == 'completed'){
            return this.todos.filter(todo => todo.completed)
        }
    }
  },
  methods:{
    addTodo: function (){
      // eslint-disable-next-line no-empty
      if(this.newTodo.trim().length == 0){
        return;
      }
        this.todos.push({
          id: this.idForTodo,
          title: this.newTodo,
          completed: false,
        })

        this.idForTodo ++;
        this.newTodo = '';
    },
    removeTodo: function (index){
      this.todos.splice(index, 1);
    },
    changeMode: function (index, title){
      this.editCache = title
      this.todos[index].editingMode = !this.todos[index].editingMode;
    },
    cancelEdit: function (todo){
      todo.title = this.editCache;
      todo.editingMode = false;
    },
    checkAllTodos: function (){
      this.todos.forEach((todo) => todo.completed = event.target.checked)
    }
  },
}
</script>

<style lang="scss">

.input-todo{
  width: 100%;
  padding: 10px 18px;
  font-size: 18px;
  margin-bottom: 16px;

  &:focus{

   }
}

.item-todo{
  margin-bottom: 12px;
  display: flex;
  align-items: center;
  justify-content: space-between;
}

.remove-button{
  cursor: pointer;
  margin-left: 14px;
  &:hover{
    color: black;
  }
}

.todo-item-label{
  padding: 10px;
  border: 1px solid white;
  margin-left: 12px;
}

.todo-item-edit{
  font-size: 24px;
  color: #2c3e50;
  margin-left: 12px;
  width: 100%;
  padding: 10px;
  border: 1px solid #ccc;
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  &:focus{
    outline: none;
  }
}

.todo-item-left{
  display: flex;
  align-items: center;
}

.completed{
  text-decoration: line-through;
  color: gray;
}

.extra-container{
  display: flex;
  align-items: center;
  justify-content: space-between;
  font-size: 16px;
  border-top: 1px solid lightgray;
  padding-top: 14px;
  margin-bottom: 14px;
}

button{
  font-size: 14px;
  background-color: white;
  appearance: none;
  &:hover{
    background: lightgreen;
  }

  &:focus{
    outline: none;
  }
}

.active{
  background: lightgreen;
}

</style>
