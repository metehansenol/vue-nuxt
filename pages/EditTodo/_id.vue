<template>
  <v-card class="d-flex pa-2" outlined tile>
    <v-container xs="12" sm="10" md="8" lg="7" xl="6">
      <v-row justify="center" align="center">
        <v-col xs="12" sm="10" md="8" lg="7" xl="6">
          <v-text-field v-model="order" label="Order" required></v-text-field>
        </v-col>
        <v-col xs="12" sm="10" md="8" lg="7" xl="6">
          <v-text-field v-model="title" label="Title" required></v-text-field>
        </v-col>
        <v-col xs="12" sm="10" md="8" lg="7" xl="6">
          <v-checkbox label="Completed" v-model="completed"></v-checkbox>
        </v-col>
        <v-col xs="12" sm="10" md="8" lg="7" xl="6">
          <v-menu
            v-model="menu"
            :close-on-content-click="false"
            transition="scale-transition"
            offset-y
            max-width="290px"
            min-width="auto"
          >
            <template v-slot:activator="{ on, attrs }">
              <v-text-field
                v-model="computedDateFormatted"
                label="Date"
                hint="MM/DD/YYYY format"
                persistent-hint
                prepend-icon="mdi-calendar"
                readonly
                v-bind="attrs"
                v-on="on"
              ></v-text-field>
            </template>
            <v-date-picker
              v-model="date"
              no-title
              @input="menu = false"
            ></v-date-picker>
          </v-menu>
        </v-col>
        <v-col xs="12" sm="10" md="8" lg="7" xl="6">
          <v-btn rounded align="center" v-on:click="PostEdit">Kaydet</v-btn>
          <v-dialog transition="dialog-bottom-transition" max-width="600">
            <template v-slot:activator="{ on, attrs }">
              <v-btn rounded color="error" v-bind="attrs" v-on="on">Sil</v-btn>
            </template>
            <template v-slot:default="dialog">
              <v-card>
                <v-toolbar color="error" dark class="text-h5"
                  >Aşağıdaki ToDo'yu Silmek İstediğinize Emin Misiniz
                  ?</v-toolbar
                >
                <v-card-text>
                  <div class="text-h5 pa-12 text-center">
                    <v-row> Order: {{ order }} </v-row>
                    <v-row> Title: {{ title }} </v-row>
                    <v-row>
                      Durumu: {{ completed ? "Tamamlanmış" : "Tamamlanmamış" }}
                    </v-row>
                    <v-row> Tarih: {{ date }} </v-row>
                  </div>
                </v-card-text>
                <v-card-actions class="d-flex justify-space-around">
                  <v-btn text color="primary" @click="dialog.value = false"
                    >İptal</v-btn
                  >
                  <v-btn
                    text
                    color="error"
                    @click="dialog.value = false"
                    v-on:click="PostDelete"
                    >Sİl</v-btn
                  >
                </v-card-actions>
              </v-card>
            </template>
          </v-dialog>
        </v-col>
      </v-row>
    </v-container>
  </v-card>
</template>

<script>
export default {
  methods: {
    PostEdit: function () {
      console.log(this.date);
      const postData = {
        completed: this.completed,
        order: this.order,
        title: this.title,
      };

      this.$axios
        .$put("https://todowebapi.herokuapp.com/items/" + this.todoID, postData)
        .then((res) => {
          this.$router.go(-1);
        });
    },
    PostDelete: function () {
      this.$axios
        .$delete("https://todowebapi.herokuapp.com/items/" + this.todoID)
        .then((res) => {
          console.log(res);
        });
    },
    formatDate(date) {
      if (!date) return null;

      const [year, month, day] = date.split("-");
      return `${month}/${day}/${year}`;
    },
    parseDate(date) {
      if (!date) return null;

      const [month, day, year] = date.split("/");
      return `${year}-${month.padStart(2, "0")}-${day.padStart(2, "0")}`;
    },
  },
  data() {
    (vm) => ({
      date: new Date().toISOString().substr(0, 10),
      dateFormatted: vm.formatDate(new Date().toISOString().substr(0, 10)),
    });
    return {
      title: "",
      todoID: -1,
      order: -1,
      completed: false,
      itemDate: null,
      menu: false,
      date: null,
    };
  },
  mounted: function () {
    const id = this.$route.params.id;
    this.$axios
      .$get("https://todowebapi.herokuapp.com/items/" + id)
      .then((item) => {
        this.todoID = item.id;
        this.title = item.title;
        this.order = item.order;
        this.date = item.createdOn.substr(0, 10);
        this.completed = item.completed;
      });
  },
  computedDateFormatted() {
    return this.formatDate(this.date);
  },
  watch: {
    date(val) {
      this.dateFormatted = this.formatDate(this.date);
    },
  },
  computed: {
    computedDateFormatted() {
      return this.formatDate(this.date);
    },
  },
};
</script>

<style scoped>
</style>