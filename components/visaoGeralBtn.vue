<template>
  <div>
    <div id="visao-geral">
      <v-card color="light-blue darken-4" @click="showAllActive('AllActive')" light width="24%" height="5em">
        <div id="ativos-icone">
          <v-icon color="white" x-large>mdi mdi-rocket</v-icon>
          <p>{{ ativos.length }} ATIVOS</p>
        </div>
      </v-card>
      <v-card
        @click="showAllActive('ActiveOperation')"
        color="green"
        width="24%"
        height="5em"
      >
        <div class="status">
          <svg height="10" width="10">
            <circle cx="5" cy="5" r="5" fill="green" />
          </svg>
          <p>EM OPERAÇÃO</p>
          <p>{{ getNumberOf("inOperation") }}</p>
        </div>
      </v-card>
      <v-card
        @click="showAllActive('ActiveAlert')"
        color="red"
        width="24%"
        height="5em"
      >
        <div class="status">
          <svg height="10" width="10">
            <circle cx="5" cy="5" r="5" fill="red" />
          </svg>
          <p>EM ALERTA</p>
          <p>{{ getNumberOf("inAlert") }}</p>
        </div>
      </v-card>
      <v-card
        light
        @click="showAllActive('ActiveDowntime')"
        color="amber"
        width="24%"
        height="5em"
      >
        <div class="status">
          <svg height="10" width="10">
            <circle cx="5" cy="5" r="5" fill="yellow" />
          </svg>
          <p>EM PARADA</p>
          <p>{{ getNumberOf("inDowntime") }}</p>
        </div>
      </v-card>
    </div>
    <div id="btns">
      <v-btn @click="showAllActive('ActiveUnit1')" class="ma-2" color="#185ef6">
        Ativos Unidade Jaguar</v-btn
      >
      <v-btn @click="showAllActive('ActiveUnit2')" class="ma-2" color="#185ef6">
        Ativos Unidade Tobias</v-btn
      >
    </div>
    <div id="tabela">
      <tabela-ativos v-if="!tbAtivos"></tabela-ativos>
      <tabela-unit-1 v-if="!tbActiveUnit1"></tabela-unit-1>
      <tabela-unit-2 v-if="!tbActiveUnit2"></tabela-unit-2>
      <tabela-alert v-if="!tbActiveAlert"></tabela-alert>
      <tabela-operation v-if="!tbActiveOperation"></tabela-operation>
      <tabela-downtime v-if="!tbActiveDowntime"></tabela-downtime>
    </div>
  </div>
</template>

<script>
import axios from "axios";
import TabelaAlert from "./tabelaAlert.vue";
import TabelaDowntime from "./tabelaDowntime.vue";
import TabelaOperation from "./tabelaOperation.vue";
import tabelaUnit1 from "./tabelaUnit1.vue";
import TabelaUnit2 from "./tabelaUnit2.vue";

export default {
  components: {
    tabelaUnit1,
    TabelaUnit2,
    TabelaAlert,
    TabelaOperation,
    TabelaDowntime,
  },
  mounted() {
    axios
      .get("https://my-json-server.typicode.com/tractian/fake-api/assets")
      .then((response) => {
        this.ativos = response.data;
      });
  },

  data() {
    return {
      tbAtivos: false,
      tbActiveUnit1: true,
      tbActiveUnit2: true,
      tbActiveAlert: true,
      tbActiveOperation: true,
      tbActiveDowntime: true,
      ativos: new Array(),
    };
  },
  methods: {
    getNumberOf(stt) {
      return this.ativos.filter((ativos) => ativos.status === stt).length;
    },
    showAllActive(name) {
      if (name === "AllActive") {
        this.tbAtivos = false;
        this.tbActiveUnit1 = true;
        this.tbActiveUnit2 = true;
        this.tbActiveAlert = true;
        this.tbActiveOperation = true;
        this.tbActiveDowntime = true;
      } else if (name === "ActiveUnit1") {
        this.tbAtivos = true;
        this.tbActiveUnit1 = false;
        this.tbActiveUnit2 = true;
        this.tbActiveAlert = true;
        this.tbActiveOperation = true;
        this.tbActiveDowntime = true;
      } else if (name === "ActiveUnit2") {
        this.tbAtivos = true;
        this.tbActiveUnit1 = true;
        this.tbActiveUnit2 = false;
        this.tbActiveAlert = true;
        this.tbActiveOperation = true;
        this.tbActiveDowntime = true;
      } else if (name === "ActiveAlert") {
        this.tbAtivos = true;
        this.tbActiveUnit1 = true;
        this.tbActiveUnit2 = true;
        this.tbActiveAlert = false;
        this.tbActiveOperation = true;
        this.tbActiveDowntime = true;
      } else if (name === "ActiveOperation") {
        this.tbAtivos = true;
        this.tbActiveUnit1 = true;
        this.tbActiveUnit2 = true;
        this.tbActiveAlert = true;
        this.tbActiveOperation = false;
        this.tbActiveDowntime = true;
      } else if (name === "ActiveDowntime") {
        this.tbAtivos = true;
        this.tbActiveUnit1 = true;
        this.tbActiveUnit2 = true;
        this.tbActiveAlert = true;
        this.tbActiveOperation = true;
        this.tbActiveDowntime = false;
      }
    },
  },
};
</script>

<style scoped>
#btns {
  display: flex;
  justify-content: space-evenly;
  align-items: center;
  margin: 1em;
}

#visao-geral {
  display: flex;
  align-items: center;
  justify-content: space-between;
}

#ativos-icone {
  display: flex;
  justify-content: center;
  align-items: center;
  padding: 1.3em;
}

#ativos-icone p {
  font-size: 1rem;
  margin: 0;
  color: white;
  font-weight: 800;
}

.status {
  display: flex;
  justify-content: center;
  align-items: center;
  padding: 1.8em;
}

.status p {
  margin: 0 0 0 1em;
  color: white;
  font-weight: 800;
}
</style>