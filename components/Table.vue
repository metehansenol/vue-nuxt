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
          >
            <td>{{ item.order }}</td>
            <td>{{ item.title }}</td>
            <td>
              <div class="link">
                <i class="cib-nuxt-js"></i>
                <NuxtLink :to="{ path: `EditTodo/${item.id}` }">Edit</NuxtLink>
              </div>
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
  },
  data() {
    return {};
  },
  methods: {
    pushITem: function (item) {
      router.push({ name: "EditTodo", params: item });
    },
    onItemClick: function (item) {
      this.$emit("todoItemSelected", item);
    },
  },
};
</script>

<style>
</style>