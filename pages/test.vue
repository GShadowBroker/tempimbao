<template>
  <v-row class="d-flex justify-center">
    <v-col class="col-6">
      <v-autocomplete
        v-model="model"
        solo
        :items="items"
        :loading="isLoading"
        hide-no-data
        label="Public APIs"
        placeholder="Start typing to search"
      ></v-autocomplete>
    </v-col>
  </v-row>
</template>

<script>
export default {
  data() {
    return {
      model: null,
      items: [
        { text: 'Adamantina', value: '01' },
        { text: 'Blumenau', value: '02' },
        { text: 'Diamantina', value: '03' },
        { text: 'Guarulhos', value: '04' },
        { text: 'São Paulo', value: '05' }
      ],
      isLoading: false,
      search: null
    }
  }
}
</script>
