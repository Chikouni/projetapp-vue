<template>
  <div class="hello" id="app">
    <div class="container">
    <b-card v-for="info in infos"
      style="max-width: 30rem;"
      class="mb-2"
      v-bind:img-src="info.fields.media_1"
      v-bind:title="info.fields.nom"
      :class="{ today : info.fields.date === currentDate }"
      border-variant="dark"
      align="center"
      footer="Ah"
      >
      <b-card-text>
        <p>
          <span class='fieldInfo'>Adresse :</span>
           {{ info.fields.adresse }} {{  info.fields.code_postal }} {{  info.fields.ville }}
        </p>
        <p>
          <span class='fieldInfo'>Date :</span>
          {{ info.fields.date | format }}
        </p>
        <p v-if='fullInfo'>
          <span class='fieldInfo'>Places :</span>
          <span v-if='info.fields.precisions_tarifs'>{{ info.fields.precisions_tarifs }}</span>
          <span v-else>Gratuit</span>
        </p>
        <b-button v-on:click='fullInfo = !fullInfo'>Afficher toute les infos</b-button>
      </b-card-text>
    </b-card>
</div>
  </div>
</template>

<script>
import axios from 'axios' // import lib
import moment from 'moment'
import 'bootstrap/dist/css/bootstrap.css'
import 'bootstrap-vue/dist/bootstrap-vue.css'
require('moment/locale/fr')

var apiURL = 'https://data.nantesmetropole.fr/api/records/1.0/search/?dataset=244400404_agenda-evenements-nantes-nantes-metropole&facet=emetteur&facet=rubrique&facet=lieu&facet=ville'
export default{
  el: '#app',
  data () {
    return {
      infos: null,
      currentDate: moment().format('YYYY-MM-DD'), // Get date of the day and format
      fullInfo: false // Bool for toggle full informations
    }
  },
  mounted () {
    axios
      .get(apiURL)
      .then(response => (this.infos = response.data.records))
  },
  filters: {
    format: function (value) {
      return moment(value).format('dddd LL')
    }
  }
}
</script>

<style>
.card.today{
  border: #4db12c solid 5px;
}
span.fieldInfo{
  font-weight: 600;
}
</style>
