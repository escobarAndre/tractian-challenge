<template>
  <v-card light class="mt-5" elevation="7">
    <v-card-title>
      <v-text-field
        light
        v-model="search"
        append-icon="mdi-magnify"
        label="Search"
        single-line
        hide-details
      ></v-text-field>
    </v-card-title>
    <v-data-table
    light
      :headers="headers"
      :items="desserts"
      :search="search"
    ></v-data-table>
  </v-card>
</template>

<script>
import axios from "axios";

export default {
  beforeMount() {
    axios
      .get("https://my-json-server.typicode.com/tractian/fake-api/users")
      .then((response) => {
        this.desserts = response.data
        console.log(this.items)
      });
  },
  data() {
    return {
      search: '',
        headers: [
          {
            text: 'Name',
            align: 'start',
            filterable: false,
            value: 'name',
          },
          { text: 'ID', value: 'id' },
          { text: 'Email', value: 'email' },
          { text: 'Unidade', value: 'unitId' },
          { text: 'Companhia', value: 'companyId' },
        ],
        desserts: []
    };
  },
  computed: {
    numberOfPages() {
      return Math.ceil(this.items.length / this.itemsPerPage);
    },
    filteredKeys() {
      return this.keys.filter((key) => key !== "Name");
    },
  },
  methods: {
    nextPage() {
      if (this.page + 1 <= this.numberOfPages) this.page += 1;
    },
    formerPage() {
      if (this.page - 1 >= 1) this.page -= 1;
    },
    updateItemsPerPage(number) {
      this.itemsPerPage = number;
    },
  },
};
</script>