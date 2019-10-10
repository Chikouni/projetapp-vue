<template>
  <div class="hello" id="app">
    <div class='btn'>
      <button v-on:click='fullInfo = !fullInfo'>Afficher toute les infos</button>
    </div>
    <ul>
    <li v-for="info in infos">
        <h1>{{ info.fields.nom }}</h1>

        <a v-bind:href="info.fields.lien_agenda">
          <img v-bind:src="info.fields.media_1" :class="{ today : info.fields.date === currentDate }"/>
        </a>
        <p>
          <span class='fieldInfo'>Adresse :</span>
           {{ info.fields.adresse }} {{  info.fields.code_postal }} {{  info.fields.ville }}
        </p>
        <p v-if='fullInfo'>
          <span class='fieldInfo'>Places :</span>
          <span v-if='info.fields.precisions_tarifs'>{{ info.fields.precisions_tarifs }}</span>
          <span v-else>Gratuit</span>
        </p>
    </li>
</ul>
  </div>
</template>

<script>
import axios from 'axios' // import lib
import moment from 'moment'

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
  filters: { // Pas eu le temps de faire le filtre / peut etre avec computed
  }
}
</script>

<style>
h1, h2 {
  font-weight: normal;
  max-width: 50%;
  margin: auto;
}

ul {
  list-style-type: none;
  padding: 0;
}

li {
  display: inline-block;
  margin: 0 10px;
}

a {
  color: #35495E;
}

img.today{
  border: #4db12c solid 5px;
}

span.fieldInfo{
  font-weight: 600;
}
p{
  max-width: 50%;
  margin: auto;
}
</style>
