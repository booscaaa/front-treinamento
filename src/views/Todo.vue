<template>
  <v-app>
    <v-app-bar color="deep-purple accent-4" dark app>
      <!-- <v-app-bar-nav-icon></v-app-bar-nav-icon> -->

      <v-toolbar-title>Tarefas</v-toolbar-title>

      <v-spacer> </v-spacer>
      <v-icon @click="dialog = true">mdi-plus</v-icon>
    </v-app-bar>
    <v-main class="px-5">
      <cgi-card v-for="(item, key) in itens" :key="key" :item="item" />
    </v-main>

    <v-dialog v-model="dialog" width="500">
      <v-card>
        <v-card-title class="headline grey lighten-2">
          Item
        </v-card-title>

        <v-card-text class="pt-5">
          <v-form ref="form">
            <v-text-field
              v-model="nome"
              :rules="regrasNome"
              label="Nome"
              required
              filled
            ></v-text-field>

            <v-textarea
              filled
              v-model="descricao"
              :rules="regrasDescricao"
              label="Descrição"
              required
            ></v-textarea>

            <v-menu
              v-model="menu"
              :close-on-content-click="false"
              :nudge-right="40"
              transition="scale-transition"
              offset-y
              min-width="290px"
            >
              <template v-slot:activator="{ on, attrs }">
                <v-text-field
                  filled
                  v-model="dateFormatted"
                  label="Data"
                  readonly
                  v-bind="attrs"
                  v-on="on"
                ></v-text-field>
              </template>
              <v-date-picker
                v-model="date"
                @input="menu = false"
              ></v-date-picker>
            </v-menu>
          </v-form>
        </v-card-text>

        <v-divider></v-divider>

        <v-card-actions>
          <v-spacer></v-spacer>
          <v-btn color="primary" text @click="save">
            Salvar
          </v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>
  </v-app>
</template>

<script>
import axios from "axios";
import CardVue from "../components/Card.vue";
export default {
  components: {
    "cgi-card": CardVue,
  },
  data: (vm) => ({
    url: "http://localhost:3333",
    itens: [],
    dialog: false,
    regrasNome: [(v) => !!v || "Preencha o nome"],
    regrasDescricao: [(v) => !!v || "Preencha a descrição"],
    nome: null,
    descricao: null,
    data: null,
    date: new Date().toISOString().substr(0, 10),
    dateFormatted: vm.formatDate(new Date().toISOString().substr(0, 10)),
    menu: false,
  }),
  computed: {
    computedDateFormatted() {
      return this.formatDate(this.date);
    },
  },
  watch: {
    date() {
      this.dateFormatted = this.formatDate(this.date);
    },
  },
  async mounted() {
    const response = await axios.get(this.url + "/item?sigla=vnb");
    this.itens = response.data ?? [];
  },
  methods: {
    save() {
      if (this.$refs.form.validate()) {
        console.log("asdasd");
      }
    },
    formatDate(date) {
      if (!date) return null;

      const [year, month, day] = date.split("-");
      return `${day}/${month}/${year}`;
    },
    parseDate(date) {
      if (!date) return null;

      const [month, day, year] = date.split("/");
      return `${year}-${month.padStart(2, "0")}-${day.padStart(2, "0")}`;
    },
  },
};
</script>
