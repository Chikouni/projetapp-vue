<template>
  <div class="hello" id="app">
    <div class="container">
    <b-form-input v-model="search" placeholder="Rechercher par quartier" class="mt-3 mb-3 searchbar"></b-form-input>
    <b-card v-for="info in filteredEvent"
      style="max-width: 30rem;"
      class="mb-2"
      v-bind:img-src="info.fields.media_1"
      v-bind:title="info.fields.nom"
      :class="{ today : info.fields.date === currentDate }"
      border-variant="dark"
      align="center"
      v-bind:footer="info.fields.date | format "> 
      <b-card-text>
        <p>
          <span class='fieldInfo'>Adresse :</span>
           {{ info.fields.adresse }} {{  info.fields.code_postal }} {{  info.fields.ville }}
        </p>
        <b-collapse v-bind:id="'collapse' + info.fields.id_manif">
          <p><span class='fieldInfo'>Places :</span>
          <span v-if='info.fields.precisions_tarifs'>{{ info.fields.precisions_tarifs }}</span>
          <span v-else>Gratuit</span>
          </p>
          <p>
            <span class='fieldInfo'>Quartier :</span>
            {{ info.fields.lieu_quartier }}
          </p>
        </b-collapse>
        <b-button v-b-toggle="'collapse' + info.fields.id_manif" class="mt-3">
          <span class="when-opened">Fermer</span><span class="when-closed">En savoir plus</span>
        </b-button>
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
      infos: [],
      currentDate: moment().format('YYYY-MM-DD'), // Get date of the day and format
      search: '' // Default string search for the filter
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
  },
  computed: {
    filteredEvent: function () {
      var matcher = new RegExp(this.search, 'i')
      return this.infos.filter(function (info) {
        return matcher.test(info.fields.lieu_quartier)
      }) // Use filter by hood
    }
  }
}
</script>

<style>
.card.today{
  border: #4db12c solid 5px;
}
.card{
  margin: auto;
}
span.fieldInfo{
  font-weight: 600;
}
.collapsed > .when-opened,
:not(.collapsed) > .when-closed {
  display: none;
}
.searchbar{
  margin: auto;
  max-width: 30rem;
}
</style>
