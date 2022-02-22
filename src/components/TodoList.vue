<template>
  <v-list>
    <TodoItem
      v-for="todo in todos"
      :key="todo.id"
      :todo="todo"
      @remove-todo="removeTodo"
    />
  </v-list>
</template>

<script>
import TodoItem from "@/components/TodoItem";

export default {
  name: "TodoList",
  components: {
    TodoItem,
  },
  props: {
    todos: {
      type: Array,
    },
  },
  methods: {
    removeTodo(todo) {
      this.$emit("remove-todo", todo);
    },
  },
};
</script>
