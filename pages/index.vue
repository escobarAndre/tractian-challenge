<template>
  <!-- App.vue -->
  <v-app>
    <v-navigation-drawer id="sidebar" app>
      <!-- SIDEBAR -->
      <img src="~/assets/logo.png" alt="" />
      <p>DESAFIO PRODUCT FRONT-END ENGINEER</p>

      <v-list dense style="margin: 1rem">
        <v-list-item
          @click="click(text)"
          v-for="([icon, text], i) in items"
          :key="i"
          link
        >
          <v-list-item-icon>
            <v-icon>{{ icon }}</v-icon>
          </v-list-item-icon>

          <v-list-item-content style="textalign: left">
            <v-list-item-title>{{ text }}</v-list-item-title>
          </v-list-item-content>
        </v-list-item>
      </v-list>
    </v-navigation-drawer>

    <v-app-bar id="header" app>
      <!-- HEADER -->
      <button></button>
    </v-app-bar>

    <!-- CONTENT -->
    <v-main id="content">
      <v-container fluid>
        <visao-geral-btn v-if="!dados"></visao-geral-btn>
        <v-btn v-if="!dados" color="white" style="color: black" class="mt-5" elevation="2">Atualizar ativos</v-btn>
        <tabela-users v-if="!user"></tabela-users>
        <v-btn v-if="!user" color="white" style="color: black" class="mt-5" elevation="2">Atualizar usuarios</v-btn>
        <div>
          <graficos-ativos></graficos-ativos>
        </div>
      </v-container> </v-main
  ></v-app>
</template>

<script>
export default {
  data() {
    return {
      user: true,
      dados: false,
      items: [
        ["mdi-chart-bar", "Ativos"],
        ["mdi-account-supervisor-circle", "Usuarios"],
        ["mdi-clock-start", "Clock-in"],
      ],
    };
  },
  methods: {
    click(name) {
      if (name === "Ativos") {
        (this.dados = !this.dados), (this.user = !this.user);
      } else if (name === "Usuarios") {
        (this.user = !this.user), (this.dados = !this.dados);
      }
    },
  },
  mounted() {},
};
</script>

<style scoped>
#header {
  background-color: #1e3a8a;
}

#sidebar {
  background-color: #185ef6;
  text-align: center;
}

#sidebar img {
  width: 160px;
  margin-top: 2em;
}

#sidebar p {
  font-size: 0.7rem;
  font-weight: 600;
  margin: 0 auto;
}

#content {
  background-color: white;
}
</style>