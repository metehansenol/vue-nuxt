<template>
  <v-card>
    <v-simple-table>
      <template>
        <thead>
          <tr>
            <th class="text-left">Order</th>
            <th class="text-left">Title</th>
            <th class="text-left">Edit</th>
          </tr>
        </thead>
        <tbody>
          <tr
            v-on:click="onItemClick(item)"
            v-for="item in tableItem"
            :key="item.id"
            :style="selectedId === item.id && rowSelection ? selectedRowStyle : {}"
          >
            <td>{{ item.order }}</td>
            <td>{{ item.title }}</td>
            <td>
              <v-btn :to="`EditTodo/${item.id}`" nuxt tile small dark color="primary">
                <v-icon left> mdi-pencil </v-icon>
                Edit
              </v-btn>
            </td>
          </tr>
        </tbody>
      </template>
    </v-simple-table>
  </v-card>
</template>

<script>
export default {
  computed: {
    editTodoItem() {
      return this.$route.params.editTodoItem;
    },
  },
  props: {
    tableItem: Array,
    rowSelection: Boolean,
    selectedRowBgColor: String
  },
  data() {
    return {
      selectedId: -1,
      selectedRowStyle: {
        color: 'black',
        backgroundColor: this.selectedRowBgColor
      }
    };
  },
  methods: {
    onItemClick: function (item) {
      this.selectedId = item.id;
      this.$emit("todoItemSelected", item);
    },
  },
};
</script>

<style scoped>
  tbody tr {
    cursor: pointer;
  }
</style>