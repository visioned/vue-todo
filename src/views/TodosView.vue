<script setup>
import { ref, watch, computed, TransitionGroup, Transition } from 'vue';
import { uid } from 'uid';
import { Icon } from '@iconify/vue';
import TodoCreator from '../components/TodoCreator.vue';
import TodoItem from '../components/TodoItem.vue';

const todoList = ref([]);
watch(
  todoList,
  () => {
    setTodoListLocalStorage();
  },
  { deep: true }
);

const todoCompleted = computed(() => {
  return todoList.value.every((todo) => todo.isCompleted === true);
});

const FetchTodoList = () => {
  const savedTodoList = JSON.parse(localStorage.getItem('todoList'));
  if (savedTodoList) {
    todoList.value = savedTodoList;
  }
};

FetchTodoList();

const setTodoListLocalStorage = () => {
  localStorage.setItem('todoList', JSON.stringify(todoList.value));
};

const createTodo = (todo) => {
  todoList.value.push({
    id: uid(),
    todo,
    isCompleted: null,
    isEditing: null,
  });
};

const toggleTodoComplete = (todoPos) => {
  todoList.value[todoPos].isCompleted = !todoList.value[todoPos].isCompleted;
};

const toggleEditTodo = (todoPos) => {
  todoList.value[todoPos].isEditing = !todoList.value[todoPos].isEditing;
};

const updateTodo = (todoVal, todoPos) => {
  todoList.value[todoPos].todo = todoVal;
};

const deleteTodo = (todoId) => {
  todoList.value = todoList.value.filter((todo) => todo.id !== todoId);
};
</script>

<template>
  <main>
    <h1>Create Todo</h1>
    <TodoCreator @create-todo="createTodo" />

    <TransitionGroup name="fade" tag="ul" class="todo-list">
      <TodoItem
        v-for="(todo, index) in todoList"
        :todo="todo"
        :index="index"
        :key="todo.id"
        @toggle-complete="toggleTodoComplete"
        @edit-todo="toggleEditTodo"
        @update-todo="updateTodo"
        @delete-todo="deleteTodo"
      />
    </TransitionGroup>
    <div class="container-msg">
      <Transition name="msg" class="todos-msg">
        <p v-if="todoList.length === 0">
          <Icon icon="noto-v1:sad-but-relieved-face" width="22" />
          <span>There are no todos yet, add one!</span>
        </p>
      </Transition>
      <Transition name="msg" class="todos-msg">
        <p v-if="todoCompleted && todoList.length > 0">
          <Icon icon="noto-v1:party-popper" width="22" />
          <span>Yay! You completed all your todos!</span>
        </p>
      </Transition>
    </div>
  </main>
</template>

<style lang="scss" scoped>
main {
  display: flex;
  flex-direction: column;
  max-width: 500px;
  width: 100%;
  margin: 0 auto;
  padding: 40px 16px;
  h1 {
    margin-bottom: 16px;
    text-align: center;
  }
  .todo-list {
    display: flex;
    flex-direction: column;
    list-style: none;
    margin-top: 24px;
    gap: 20px;
  }
  .todos-msg {
    display: flex;
    align-items: center;
    justify-content: center;
    gap: 8px;
    margin-top: 24px;
    z-index: 1;
  }
}

.fade-move,
.fade-enter-active,
.fade-leave-active {
  // declaring transition
  transition: all 0.5s cubic-bezier(0.55, 0, 0.1, 1);
}

/* 2. declare enter from and leave to state */
.fade-leave-to {
  // leaving state
  opacity: 0;
  transform: scale(0.9) translateX(10px);
}

.fade-enter-active {
  // during state
  opacity: 0;
  transform: scale(1.01) translateX(-10px);
}
.fade-enter-to {
  // entering state
  opacity: 1;
  transform: scale(1) translateX(0);
}
.fade-move {
  position: absolute;
}
.msg-enter-active {
  transition: all 0.5s cubic-bezier(0.55, 0, 0.1, 1);
  transition-delay: 0.5s;
}
.msg-enter-from {
  opacity: 0;
}
.msg-leave-active {
  opacity: 0;
}
</style>
