<template>
  <v-list-item
    :class="{
      complete: todo.complete,
    }"
  >
    <v-list-item-content>
      <v-list-item-title v-text="todo.title"></v-list-item-title>
      <v-list-item-subtitle
        v-text="todo.description"
        class="mt-1"
      ></v-list-item-subtitle>
    </v-list-item-content>

    <v-list-item-action>
      <v-tooltip v-if="!todo.complete" left color="primary">
        <template v-slot:activator="{ on, attrs }">
          <v-btn
            v-bind="attrs"
            v-on="on"
            icon
            @click.prevent="removeTodo(todo)"
          >
            <v-icon color="red lighten-1">mdi-delete</v-icon>
          </v-btn>
        </template>
        <span>Завершить!</span>
      </v-tooltip>
      <v-tooltip v-else left color="primary">
        <template v-slot:activator="{ on, attrs }">
          <v-btn
            v-bind="attrs"
            v-on="on"
            icon
            @click.prevent="removeTodo(todo)"
          >
            <v-icon color="green lighten-1">mdi-replay</v-icon>
          </v-btn>
        </template>
        <span>Вернуть!</span>
      </v-tooltip>
    </v-list-item-action>
  </v-list-item>
</template>

<script>
export default {
  name: "TodoItem",
  props: {
    todo: {
      type: Object,
      require: true,
    },
  },
  methods: {
    removeTodo(todo) {
      this.$emit("remove-todo", todo);
    },
  },
};
</script>

<style lang="scss" scoped>
.v-list-item {
  $self: &;
  position: relative;
  &:not(:last-of-type) {
    border-bottom: 1px solid rgba(0, 0, 0, 0.42);
  }
  &.complete {
    background-color: #eee;
    &::after {
      content: "";
      position: absolute;
      top: 50%;
      left: 1rem;
      height: 1px;
      min-height: unset;
      background-color: rgba(0, 0, 0, 0.21);
    }
    #{$self}__content {
      text-decoration: line-through;
      opacity: 0.5;
      filter: blur(0.5px);
    }
  }
}
</style>
