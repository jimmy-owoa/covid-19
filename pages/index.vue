<template>
  <div>
    <v-row>
      <v-col cols="12" sm="12">
        <v-combobox
          v-model="item"
          :items="countries"
          label="Seleccione un país a consultar"
          :loading="loading"
          @change="getDataCountry()"
        ></v-combobox>
      </v-col>
      <v-col v-if="results" cols="12" sm="12">
        <div class="headline text-md-center">{{results.country}}</div>
        <v-simple-table>
          <template v-slot:default>
            <tbody>
              <tr>
                <td>Casos</td>
                <td>{{ results.cases }}</td>
              </tr>
              <tr>
                <td>Casos hoy</td>
                <td>{{results.todayCases}}</td>
              </tr>
              <tr>
                <td>Muertos</td>
                <td>{{results.deaths}}</td>
              </tr>
              <tr>
                <td>Muertos hoy</td>
                <td>{{results.todayDeaths}}</td>
              </tr>
              <tr>
                <td>Recuperados</td>
                <td>{{results.recovered}}</td>
              </tr>
              <tr>
                <td>Activos</td>
                <td>{{results.active}}</td>
              </tr>
              <tr>
                <td>Críticos</td>
                <td>{{results.critical}}</td>
              </tr>
              <tr>
                <td>Casos cada 1 millón</td>
                <td>{{results.casesPerOneMillion}}</td>
              </tr>
              <tr>
                <td>Muertes cada 1 millón</td>
                <td>{{results.deathsPerOneMillion}}</td>
              </tr>
              <tr>
                <td>Tests totales</td>
                <td>{{results.totalTests}}</td>
              </tr>
              <tr>
                <td>Test cada 1 millón</td>
                <td>{{results.testsPerOneMillion}}</td>
              </tr>
            </tbody>
          </template>
        </v-simple-table>
      </v-col>
    </v-row>
  </div>
</template>

<script>
import Logo from "~/components/Logo.vue";
import VuetifyLogo from "~/components/VuetifyLogo.vue";

export default {
  components: {
    Logo,
    VuetifyLogo
  },
  data() {
    return {
      countries: [],
      loading: false,
      item: null,
      info: null,
      results: ""
    };
  },
  async mounted() {
    try {
      this.loading = true;
      const res = await this.$axios.get(
        `https://coronavirus-19-api.herokuapp.com/countries`
      );
      this.info = res.data;
      this.countries = res.data.map(c => c.country);
      this.loading = false;
    } catch (e) {
      console.log(e);
    }
  },
  methods: {
    getDataCountry() {
      this.results = this.info.find(c => c.country === this.item);
      console.log(this.results);
    }
  }
};
</script>
