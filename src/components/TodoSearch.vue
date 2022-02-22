<template>
  <v-card class="pa-5">
    <v-flex class="d-flex align-center">
      <v-text-field
        type="text"
        :value="search"
        @input="updateSearch"
        label="Поиск..."
      ></v-text-field>
      <v-btn-toggle class="ml-5">
        <v-btn
          v-for="button in buttonsList"
          :key="button.title"
          small
          :color="button.active ? 'primary' : 'mormal'"
          @click.prevent="updateFilter(button.title)"
        >
          {{ button.title }}
        </v-btn>
      </v-btn-toggle>
    </v-flex>
  </v-card>
</template>

<script>
export default {
  name: "TodoSearch",
  props: {
    search: {
      type: String,
    },
    filter: {
      type: String,
    },
  },
  data: () => ({
    buttons: [
      {
        title: "Все",
        active: false,
      },
      {
        title: "Активные",
        active: false,
      },
      {
        title: "Выполненные",
        active: false,
      },
    ],
  }),
  methods: {
    updateSearch: function (e) {
      this.$emit("search-field", e);
    },
    updateFilter: function (buttonTitle) {
      this.$emit("update-filter", buttonTitle);
    },
  },
  computed: {
    buttonsList: function () {
      return this.buttons.map((button) => {
        if (button.title === this.filter) {
          button.active = true;
        } else {
          button.active = false;
        }
        return button;
      });
    },
  },
};
</script>

<style lang="scss" scoped>
.v-btn-toggle > .v-btn.v-btn--active {
  color: #fff;
}
</style>
