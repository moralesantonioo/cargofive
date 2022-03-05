<template>
  <div class="container">
    <b-table striped hover :items="data" :current-page="currentPage" :per-page="perPage" ></b-table>
    <b-pagination @change="nextData($event)" v-model="currentPage" :per-page="perPage" :total-rows="rows" pills size="sm" align="center"></b-pagination>
  </div>
  
</template>

<script>
import axios from 'axios'

export default {
  name: 'Home',
  data() {
    return {
      data: [],
      perPage: null,
      currentPage: null,
      last_page: null,
      total: null
    }
  },

  mounted(){
    this.cargofiveData(+1);
  },

  methods: {
    async cargofiveData(page){
      await axios.get(`http://apitest.cargofive.com/api/ports?page=${page}`).then(response => {
        this.data = response.data.data
        this.current_page = response.data.meta.current_page
        //this.last_page = response.data.meta.last_page
        this.perPage = response.data.meta.per_page
        this.total = response.data.meta.total

        console.log(response.data, 'data')
        //console.log(this.last_page, 'last_page')
        //console.log(this.perPage, 'perPage')
        //console.log(this.total, 'total')
        //console.log(this.current_page, 'current_page')
      })
      .catch(error => {
        console.error(error)
      })
    },
    async nextData(numberPage){
      await this.cargofiveData(numberPage)
      //console.log(pagina, 'entesa')
    }
  },

  computed: {
    rows() {
      return this.total
    }
  }
}
</script>
