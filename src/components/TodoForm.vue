<template>
  <v-dialog v-model="dialog" width="500">
    <template v-slot:activator="{ on, attrs }">
      <v-btn color="white" v-bind="attrs" v-on="on">
        <v-icon color="primary">mdi-plus</v-icon>
        <div class="primary--text">Добавить</div>
      </v-btn>
    </template>

    <v-card>
      <v-card-title class="text-h7"> Новая запись. </v-card-title>
      <v-card-text>
        <v-form ref="todoForm" @submit.prevent="addTodo">
          {{ propsName }}
          <v-text-field
            v-model="todo.title"
            label="Заголовок"
            :rules="titleRules"
          ></v-text-field>
          <v-textarea
            v-model="todo.description"
            label="Описание"
            rows="2"
            maxlength="80"
            no-resize
            :rules="titleRules"
          ></v-textarea>
          <v-btn type="submit" color="primary" elevation="5" class="mt-3"
            >Сохранить</v-btn
          >
        </v-form>
      </v-card-text>
    </v-card>
  </v-dialog>
</template>

<script>
export default {
  name: "TodoForm",
  props: {
    propsName: {
      type: String,
    },
  },
  data: () => ({
    todo: {
      title: "",
      description: "",
    },
    dialog: false,
    titleRules: [(v) => !!v || "Необходимо заполнить поле"],
    descriptionRules: [(v) => !!v || "Необходимо заполнить поле"],
  }),
  methods: {
    addTodo() {
      if (this.$refs.todoForm.validate()) {
        this.todo.id = Date.now();
        this.todo.complete = false;
        this.$emit("add-todo", this.todo);
        this.todo = {
          title: "",
          description: "",
        };
        this.$refs.todoForm.reset();
        this.dialog = false;
      }
    },
  },
};
</script>
