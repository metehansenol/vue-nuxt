<template>
  <v-card class="d-flex pa-2" outlined tile>
    <v-container xs="12" sm="10" md="8" lg="7" xl="6">
      <v-row justify="center" align="center">
        <v-col>
          <div class="text-center">
            <logo />
            <vuetify-logo />
          </div>

          <div>
            <v-row class="justify-center">
              <v-col xs="12" sm="10" md="7" lg="6" xl="5">
                <Table
                  :tableItem="todoItems"
                  v-on:todoItemSelected="itemSelected"
                  v-on:click.prevent="tableClick(item)"
                  :row-selection="true"
                  selected-row-bg-color="#f77f00"
                />
              </v-col>
            </v-row>
            <v-row class="justify-center">
              <v-col xs="8" sm="6" md="5" lg="4" xl="3">
                <v-text-field
                  label="Title"
                  v-model="todoTitle"
                  :todoTitle="rules"
                  required
                  hide-details="auto"
                ></v-text-field>
              </v-col>

              <v-col xs="3" sm="2" md="2" lg="1" xl="1">
                <v-btn
                  depressed
                  rounded
                  medium
                  v-on:click="AddTodo"
                  :color="isUpdating ? 'primary' : ''"
                >
                  Add
                </v-btn>
              </v-col>
            </v-row>
          </div>
        </v-col>
      </v-row>
    </v-container>
  </v-card>
</template>

<script>
import Logo from "~/components/Logo.vue";
import VuetifyLogo from "~/components/VuetifyLogo.vue";
import Table from "~/components/Table.vue";

export default {
  components: {
    Logo,
    VuetifyLogo,
    Table,
  },
  data: function () {
    return {
      todoItems: [],
      todoSelected: [],
      todoTitle: "",
      rules: [
        (value) => !!value || "Required.",
        (value) => value || value.length >= 3 || "Min 3 characters",
      ],
    };
  },
  mounted: async function () {
    const data = await this.$axios.$get(
      "https://todowebapi.herokuapp.com/items"
    );

    this.todoItems = data;
    console.log(data);
  },
  computed: {
    isUpdating: function () {
      return this.todoItemID > -1;
    },
  },
  methods: {
    itemSelected(item) {
      this.todoSelected = item;
      this.todoTitle = item.title;
    },
    tableClick(item) {
      this.todoSelected = {
        id: item.id,
        title: item.title,
        order: item.order,
      };
    },
    AddTodo: async function () {
      if (this.rules) {
        let todoPost = {
          title: this.todoTitle,
        };
        const res = await this.$axios.$post(
          "https://todowebapi.herokuapp.com/items",
          todoPost
        );

        this.todoItems.push(res);
        this.todoTitle = null;
      }
    },
  },
};
</script>
