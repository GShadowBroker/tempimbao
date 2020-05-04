<template>
  <v-app light>
    <v-app-bar app color="primary" light elevate-on-scroll>
      <v-app-bar-nav-icon left @click.stop="drawer = !drawer" />
      <v-toolbar-title class="yellow--text">
        TEMPIM<span class="blue--text text--darken-4 font-weight-black"
          >BÃO</span
        >
      </v-toolbar-title>
      <v-spacer></v-spacer>
      <v-btn small class="mx-2" outlined>login</v-btn>
      <v-btn small class="secondary mx-2">sign up</v-btn>
    </v-app-bar>

    <v-navigation-drawer
      v-model="drawer"
      elevation="2"
      class="primary--text"
      light
      app
    >
      <v-list-item class="secondary">
        <v-list-item-content>
          <v-list-item-title class="title yellow--text">
            TEMPIM<span class="cyan--text font-weight-black">BÃO</span>
          </v-list-item-title>
          <v-list-item-subtitle class="grey--text text--lighten-2">{{
            getQuote()
          }}</v-list-item-subtitle>
        </v-list-item-content>
      </v-list-item>

      <v-divider></v-divider>

      <v-list nav class="secondary--text">
        <v-list-item
          v-for="(item, index) in items"
          :key="index"
          link
          :to="item.to"
        >
          <v-list-item-title active-class="orange">
            <v-icon left>{{ item.icon }}</v-icon>
            <span>{{ item.title }}</span>
          </v-list-item-title>
        </v-list-item>
      </v-list>
    </v-navigation-drawer>

    <v-content class="grey lighten-2">
      <v-container>
        <nuxt />
      </v-container>
    </v-content>

    <v-footer color="primary lighten-2" light absolute app>
      <span>&copy; {{ new Date().getFullYear() }} - TempimBão</span>
    </v-footer>
  </v-app>
</template>

<script>
export default {
  data() {
    return {
      drawer: false,
      items: [
        {
          icon: 'mdi-apps',
          title: 'Previsão',
          to: '/'
        },
        {
          icon: 'mdi-chart-bubble',
          title: 'Sobre',
          to: '/sobre'
        },
        {
          icon: 'mdi-chart-bubble',
          title: 'Teste',
          to: '/test'
        }
      ],
      subtitles: [
        'E ai? Será que vai chover?',
        'Será que vai dar praia?',
        'Saudades do calor, né minha filha?',
        'Devo levar o guarda-chuva?',
        'Devo levar o casaco?',
        'Será que vai dar sereno?',
        'Será que vai gear amanhã?',
        'Já posso fazer anjinho na neve?',
        'Já posso fritar ovo no asfalto?',
        'Favorável pra sair com o crush?',
        'Tempo favorável pra vender picolé?',
        'Rola levar o doguinho pra passear?'
      ]
    }
  },
  methods: {
    getQuote() {
      return this.subtitles[Math.floor(Math.random() * this.subtitles.length)]
    }
  }
}
</script>
