<template>
  <v-row justify="space-around">
    <v-col cols="auto">
      <v-dialog transition="dialog-top-transition" max-width="1000">
        <template @click="click" v-slot:activator="{ on, attrs }">
          <v-btn color="blue darken-1" v-bind="attrs" v-on="on">
            <i class="mdi mdi-arrow-top-left-bottom-right"></i>
          </v-btn>
        </template>

        <template v-slot:default="dialog">
          <v-card light>
            <v-toolbar color="primary" dark
              ><p id="titulo">{{ ativos.name }}</p></v-toolbar
            >
            <v-card-text class="pt-5">
              <div id="cols">
                <div style="paddingright: 2.2rem" id="col-1">
                  <img
                    style="borderradius: 0.5rem"
                    id="image"
                    :src="`${ativos.image}`"
                    alt=""
                  />
                </div>
                <div>
                  <p>
                    Sensor: <b>{{ ativos.sensors.toString() }}</b>
                  </p>
                  <p>
                    Model: <b>{{ ativos.model }}</b>
                  </p>
                  <p>
                    Saúde: <b>{{ ativos.healthscore }}%</b>
                  </p>
                  <p>
                    Temperatura Máxima:
                    <b>{{ ativos.specifications.maxTemp }}ºC</b>
                  </p>
                  <p>
                    Rotações por Minuto: <b>{{ ativos.specifications.rpm }}</b>
                  </p>
                  <p>
                    Força (kWh): <b>{{ ativos.specifications.power }}</b>
                  </p>
                  <p>
                    Responsável: <b>{{ func }}</b>
                  </p>
                  <div id="group-select">
                    <v-select
                      @click="allUsers()"
                      dense
                      :items="items"
                      label="Delegar responsável"
                      outlined
                      id="select"
                      v-model="responsavel"
                    ></v-select>
                    <v-btn id="btn-select" @click="enviar(responsavel)" elevation="2">OK</v-btn>
                  </div>
                </div>
                <div>
                  <v-card
                    :style="colorStatus(ativos.status)"
                    elevation="4"
                    id="card-status"
                    @click="allUsers()"
                  >
                    <i class="mdi mdi-alert mr-5"></i>
                    <p>{{ showStatus(ativos.status) }}</p>
                  </v-card>
                  <v-card @click="rpm()" elevation="4" id="card-dados">
                    <div style="textalign: center; margin: 1em">
                      <h3 style="color: #03a9f4">Dados da Coleta</h3>
                    </div>
                    <p>
                      Total de Coletas:
                      <b>{{ ativos.metrics.totalCollectsUptime }}</b>
                    </p>
                    <p>
                      Total de Horas Coletadas:
                      <b>{{ ativos.metrics.totalUptime.toFixed(2) }}</b>
                    </p>
                    <p style="fontsize: 0.7rem">
                      Data da Última Coleta: <b>{{ data() }}</b>
                    </p>
                  </v-card>
                </div>
              </div>
            </v-card-text>
            <v-card-actions class="justify-end">
              <v-btn @click="att, alert('Ativo atualizado.')" text>Atualizar</v-btn>
              <v-btn text @click="dialog.value = false">Close</v-btn>
            </v-card-actions>
          </v-card>
        </template>
      </v-dialog>
    </v-col>
  </v-row>
</template>

<script>
import axios from "axios";

export default {
  props: {
    activeName: {
      type: Number,
    },
  },
  data() {
    return {
      ativos: new Array(),
      users: new Array(),
      items: [],
      func: 'Sem responśavel',
      responsavel: ''

    };
  },
  beforeCreate() {},

  mounted() {
    this.att()
  },
  methods: {
    alert(mensagem) {
      alert(mensagem)
    },
    att() {
      axios
      .get(
        `https://my-json-server.typicode.com/tractian/fake-api/assets/${this.activeName}`
      )
      .then((response) => {
        this.ativos = response.data;
      });
      

    axios
      .get(`https://my-json-server.typicode.com/tractian/fake-api/users`)
      .then((response) => {
        this.users = response.data;
      });
    },
    enviar(content) {
      this.func = this.responsavel
      const body = {
        responsavel: this.responsavel,
      };
      axios
        .put(`https://my-json-server.typicode.com/tractian/fake-api/assets/${this.activeName}`, {
          body,
        })
        .then((res) => {
          alert("Delegação concluída! Veja o console."), console.log(res);
        });
    },
    allUsers() {
      const list = this.users.map((user) => {
        return user.name;
      });

      this.items = list;
    },
    showStatus(status) {
      if (status === "inAlert") {
        return "ALERTA";
      } else if (status === "inOperation") {
        return "OPERAÇÃO";
      } else if (status === "inDowntime") {
        return "PARADA";
      }
    },
    colorStatus(status) {
      if (status === "inAlert") {
        return "backgroundColor: red; color: white";
      } else if (status === "inDowntime") {
        return "backgroundColor: yellow; color: black";
      } else if (status === "inOperation") {
        return "backgroundColor: green; color: white";
      }
    },
    data() {
      const data = this.ativos.metrics.lastUptimeAt;
      return `${data.slice(8, 10)}/${data.slice(5, 7)}/${data.slice(
        0,
        4
      )} às ${data.slice(11, 20)} `;
    },
  },
};
</script>

<style scoped>
#image {
  width: 250px;
  height: 250px;
}

#cols {
  display: grid;
  grid-template-columns: 30% 40% 30%;
}

#card-status {
  background-color: crimson;
  width: 100%;
  height: 5rem;
  color: #ffb300;
  display: flex;
  justify-content: center;
  padding: 1.8rem;
  font-size: 2rem;
  text-align: center;
}

#card-dados {
  padding: 0.2rem 0.5rem 0 0.5rem;
  margin-top: 1rem;
}

#titulo {
  text-align: center;
  width: 100%;
  margin: 0;
  font-size: 2rem;
  font-weight: 500;
}

#select {
  width: 5rem;
}

#group-select {
  display: flex;
  width: 80%;
}

#btn-select{
  height: 2.5rem;
  margin: 0 .5rem;
}
</style>