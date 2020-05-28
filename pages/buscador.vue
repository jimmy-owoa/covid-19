<template>
  <div>
    <v-row>
      <v-col cols="12" sm="12">
        <v-text-field label="País" placeholder="chile" v-model="country" @input="search()"></v-text-field>
      </v-col>
      <v-col>
        <v-card v-for="(data, i) in results" :key="i">
          {{data.key}}
        </v-card>
        {{results}}
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
      country: "",
      results: "",
      loading: false
    };
  },
  methods: {
    async search() {
      console.log(this.country);
      if (this.country.length > 2) {
        try {
          this.loading = true;
          const res = await this.$axios.get(
            `https://coronavirus-19-api.herokuapp.com/countries/${this.country}`
          );
          this.results = res.data;
          this.loading = false;
        } catch (e) {
          console.log(e);
        }
      }
    }
  }
};
</script>
