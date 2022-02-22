<template>
  <v-app>
    <v-app-bar app color="primary" dark>
      <v-container>
        <v-flex d-flex>
          <div class="text-h6 text-sm-h5">Vue Todos</div>
          <v-spacer></v-spacer>
          <TodoForm @add-todo="addTodo" />
        </v-flex>
      </v-container>
    </v-app-bar>
    <v-main>
      <v-container class="py-2 py-sm-5">
        <v-row>
          <v-col cols="12" md="8" lg="6" class="mx-auto">
            <div class="mt-5">
              <TodoSearch
                @search-field="editSearchField"
                @update-filter="updateFilter"
                :search="search"
                :filter="filter"
              />
            </div>
            <div class="mt-2">
              <TodoList :todos="normalTodos" @remove-todo="removeTodo" />
            </div>
          </v-col>
        </v-row>
      </v-container>
    </v-main>
  </v-app>
</template>

<script>
import TodoForm from "@/components/TodoForm";
import TodoList from "@/components/TodoList";
import TodoSearch from "@/components/TodoSearch";

export default {
  name: "App",
  components: {
    TodoForm,
    TodoList,
    TodoSearch,
  },
  data: () => ({
    todos: [],
    search: "",
    filter: "Все",
  }),
  mounted() {
    this.todos = JSON.parse(localStorage.getItem("todos")) || [];
  },
  methods: {
    saveTodos() {
      localStorage.setItem("todos", JSON.stringify(this.todos));
    },
    addTodo(todo) {
      this.todos.push(todo);
      this.saveTodos();
    },
    removeTodo(todo) {
      this.todos = this.todos.map((el) => {
        if (todo == el) {
          el.complete = !el.complete;
        }
        return el;
      });

      this.saveTodos();
    },
    editSearchField(searchField) {
      this.search = searchField;
    },
    updateFilter(updateFilterTitle) {
      this.filter = updateFilterTitle;
    },
    searchTodos(items) {
      return items.filter((todo) =>
        todo.title.toLowerCase().includes(this.search.toLowerCase())
      );
    },
    filteredTodos(items) {
      switch (this.filter) {
        case "Все":
          return items;
        case "Активные":
          return items.filter((todo) => !todo.complete);
        case "Выполненные":
          return items.filter((todo) => todo.complete);
      }
    },
  },
  computed: {
    normalTodos: function () {
      return this.filteredTodos(this.searchTodos(this.todos));
    },
  },
};
</script>
