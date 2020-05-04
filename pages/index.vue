<template>
  <v-container>
    <v-row class="d-flex justify-center">
      <v-col class="col-12 col-sm-10 col-md-8">
        <v-autocomplete
          v-model="city_name"
          :loading="loading"
          :items="items"
          :search-input.sync="search"
          cache-items
          solo
          label="Nome da cidade"
          prepend-inner-icon="mdi-magnify"
          clearable
          item-text="municipio.nome"
        ></v-autocomplete>
      </v-col>
    </v-row>

    <v-row>
      <v-col class="d-flex justify-center">
        <v-btn :disabled="!city_name" @click="handleSubmit">
          <v-icon left>mdi-autorenew</v-icon>
          Atualizar cidade
        </v-btn>
      </v-col>
    </v-row>

    <v-row class="d-flex justify-center">
      <v-col class="col-12 col-sm-10 col-md-8">
        <v-card v-if="weather.city" class="mx-auto" max-width="600">
          <v-list-item two-line>
            <v-list-item-content>
              <v-list-item-title class="headline secondary--text">{{
                weather.city
              }}</v-list-item-title>
              <v-list-item-subtitle
                >Última atualização: {{ weather.date }} às {{ weather.time }}
              </v-list-item-subtitle>
              <v-list-item-title
                class="headline secondary--text pt-4 text-center"
                >{{ weather.description }}</v-list-item-title
              >
            </v-list-item-content>
          </v-list-item>

          <v-card-text>
            <v-row align="center">
              <v-col
                class="display-3 font-weight-bold secondary--text text-center"
                cols="6"
              >
                {{ weather.temp }}&deg;C
              </v-col>
              <v-col class="d-flex justify-center" cols="6">
                <v-img
                  :src="getIcon(weather.condition_code, weather.currently)"
                  :alt="weather.condition_slug"
                  max-width="92"
                ></v-img>
              </v-col>
            </v-row>
          </v-card-text>

          <v-list-item>
            <v-list-item-icon>
              <v-icon>mdi-send</v-icon>
            </v-list-item-icon>
            <v-list-item-subtitle>{{
              weather.wind_speedy
            }}</v-list-item-subtitle>
          </v-list-item>

          <v-list-item>
            <v-list-item-icon>
              <v-icon>mdi-cloud-download</v-icon>
            </v-list-item-icon>
            <v-list-item-subtitle>{{ weather.humidity }}%</v-list-item-subtitle>
          </v-list-item>

          <v-slider
            v-model="time"
            :max="6"
            :tick-labels="labels"
            class="mx-4"
            ticks
          ></v-slider>

          <v-list class="transparent">
            <v-list-item v-for="item in weather.forecast" :key="item.date">
              <v-list-item-title>{{ item.date }}</v-list-item-title>

              <v-list-item-subtitle>
                <span>{{ item.description }}</span>
              </v-list-item-subtitle>

              <v-list-item-subtitle class="text-right">
                {{ item.max }}º - {{ item.min }}º
              </v-list-item-subtitle>
            </v-list-item>
          </v-list>

          <v-divider></v-divider>

          <v-card-actions>
            <v-btn text>Dados completos</v-btn>
          </v-card-actions>
        </v-card>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
export default {
  asyncData({ params, $axios }) {
    return $axios
      .$get(`https://servicodados.ibge.gov.br/api/v1/localidades/distritos`)
      .then((res) => {
        return { autocomplete: res }
      })
  },
  data() {
    return {
      valid: true,
      city_name: '',
      apiKey: process.env.weatherAppId,
      weather: {},
      labels: ['DOM', 'SEG', 'TER', 'QUA', 'QUI', 'SEX', 'SÁB'],
      time: 0,
      search: null,
      select: null,
      loading: false,
      items: []
    }
  },
  watch: {
    search(val) {
      val && val !== this.select && this.querySelections(val)
    }
  },
  mounted() {
    if (localStorage.weather) {
      this.weather = JSON.parse(localStorage.weather)
    }
  },
  methods: {
    handleSubmit() {
      this.$axios
        .$get(
          `https://api.hgbrasil.com/weather?format=json-cors&key=${this.apiKey}&city_name=${this.city_name}`
        )
        .then((res) => {
          this.weather = res.results
          localStorage.weather = JSON.stringify(this.weather)
          this.city_name = ''
        })
    },
    querySelections(v) {
      this.loading = true
      // Simulated ajax query
      this.items = this.autocomplete.filter((e) => {
        return (e.municipio.nome || '')
          .toLowerCase()
          .includes((v || '').toLowerCase())
      })
      this.loading = false
    },
    getIcon(code, currently) {
      const iconCodes = [
        't01',
        't02',
        's05',
        't04',
        't04',
        's04',
        's04',
        's04',
        'a01',
        'd01',
        's02',
        'd01',
        'd01',
        's01',
        's03',
        's05',
        's02',
        's05',
        's03',
        'a04',
        'a05',
        'a03',
        'a04',
        's05',
        's05',
        'c04',
        'c04',
        'c01',
        'c03',
        'c02',
        'c02',
        'c01',
        'c01',
        'c01',
        'c02',
        'd01',
        'c01',
        'r05',
        't04',
        't04',
        'r05',
        's02',
        's02',
        's03',
        'c02',
        'r01',
        'd03',
        'r03',
        'u00'
      ]

      if (currently === 'dia') {
        return (
          'https://www.weatherbit.io/static/img/icons/' +
          iconCodes[code] +
          'd.png'
        )
      } else {
        return (
          'https://www.weatherbit.io/static/img/icons/' +
          iconCodes[code] +
          'n.png'
        )
      }
    }
  }
}
</script>
