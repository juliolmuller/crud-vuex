<template>
  <div class="todos-cards">
    <legend>
      <span>
        Dê duplo-clique para marcar a tarefa como completa/incompleta
      </span>
      <span>
        <span class="incomplete-box"></span> = Incompleta
      </span>
      <span>
        <span class="complete-box"></span> = Completa
      </span>
    </legend>

    <div class="todos">
      <div
        v-for="todo in todos"
        :key="todo.id"
        :class="['todo', {'is-complete':todo.completed}]"
        @dblclick="toggleComplete(todo)"
      >
        {{ todo.title }}
        <i class="fa fa-trash-o" @click="deleteTodo(todo)"></i>
      </div>
    </div>
  </div>
</template>

<script>
import { computed } from 'vue'
import { useStore } from 'vuex'

export default {
  name: 'Todos',

  async setup() {
    const store = useStore()
    const todos = computed(() => store.getters['todos/todos'])

    function deleteTodo(todo) {
      if (confirm('Tem certeza que quer excluir esta tarefa?')) {
        store.dispatch('todos/deleteTodo', todo.id)
      }
    }

    function toggleComplete(todo) {
      store.dispatch('todos/updateTodo', {
        ...todo,
        completed: !todo.completed,
      })
    }

    await store.dispatch('todos/fetchTodos')

    return {
      todos,
      deleteTodo,
      toggleComplete,
    }
  },
}
</script>

<style>
.todos-cards legend {
  display: flex;
  justify-content: space-around;
  margin-top: 2rem;
}

.todos-cards .incomplete-box {
  display: inline-block;
  width: 10px;
  height: 10px;
  background: #41b883;
}

.todos-cards .complete-box {
  display: inline-block;
  width: 10px;
  height: 10px;
  background: #35495e;
}

.todos-cards .todos {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  grid-gap: 1rem;
  margin-top: 1rem;
}

.todos-cards .todo {
  position: relative;
  padding: 1rem;
  border: 1px solid #ccc;
  border-radius: 5px;
  background: #41b883;
  text-align: center;
  user-select: none;
  cursor: pointer;
}

.todos-cards .is-complete {
  background: #35495e;
  color: #fff;
}

.todos-cards i {
  cursor: pointer;
  position: absolute;
  right: 10px;
  bottom: 10px;
  color: #fff;
}

.todos-cards i:hover {
  color: #000;
}

@media (max-width: 600px) {
  .todos-cards .todos {
    grid-template-columns: 1fr;
  }
}
</style>
