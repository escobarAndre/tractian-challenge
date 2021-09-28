<template>
  <v-card light elevation="7">
    <v-simple-table dense>
      <template v-slot:default>
        <thead>
          <tr>
            <th @click="tbAtivos != true" class="text-center">+Info</th>
            <th class="text-center">Nome</th>
            <th class="text-center">Sensor</th>
            <th class="text-center">Status</th>
            <th class="text-center">Saúde (%)</th>
            <th class="text-center">Temperatura Max. (ºC)</th>
            <th class="text-center">RPM</th>
            <th class="text-center">Força (kWh)</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="item in activeOperation" :key="item.name">
            <td><ativo :activeName="item.id"></ativo></td>
            <td class="td-ativos">{{ item.name }}</td>
            <td class="td-ativos">{{ item.sensors.toString() }}</td>
            <td class="td-ativos">
              <p :style="showStatus(item.status)">{{changeStatus(item.status)  }}</p>
            </td>
            <td class="td-ativos">{{ item.healthscore }}%</td>
            <td class="td-ativos">{{ item.specifications.maxTemp }}ºC</td>
            <td class="td-ativos">{{ item.specifications.rpm }}</td>
            <td class="td-ativos">{{ item.specifications.power }}</td>
          </tr>
        </tbody>
      </template>
    </v-simple-table>
  </v-card>
</template>

<script>
import axios from "axios";

export default {
  data() {
    return {
      activeOperation: new Array
    };
  },
  mounted() {
    axios
      .get("https://my-json-server.typicode.com/tractian/fake-api/assets")
      .then((response) => {
        const ativos = response.data;
        this.activeOperation = ativos.filter(active => active.status === 'inOperation')
      });
  },
  methods: {
    showStatus(status) {
      if (status === "inAlert") {
        return "color: red; padding-top: 1em";
      } else if (status === "inDowntime") {
        return "color: black; padding-top: 1em";
      } else if (status === "inOperation") {
        return "color: green; padding-top: 1em";
      }
    },
    changeStatus(status) {
            if (status === 'inAlert') {
                return 'Em alerta'
            } else if (status === 'inOperation') {
                return 'Em operação'
            } else if ( status === 'inDowntime') {
                return 'Em parada'
            }
        }, 
  },
};
</script>

<style>
.red {
  background-color: red;
}
.yellow {
  background-color: black;
}
.green {
  background-color: black;
}

.td-ativos {
  text-align: center;
}

.th-ativos {
  text-align: center;
}

#ativos {
  background-color: #f5f5f5;
  color: black;
}

.card {
  width: 85%;
  height: 85%;
}
</style>