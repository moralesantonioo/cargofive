<template>
  <div class="container">
    <b-row class="filters"> <!-- inputs para realizar los filtros, se hace uso de un v-select y un v-input, css Bootstrap Vue  -->
      <h1 class="title">Filtros:</h1>
      <b-col>
        <span>Seleccione opción a filtrar:</span>
        <b-form-select v-model="selected" :options="options" class="v-select-style"></b-form-select>
      </b-col>
      <b-col>
        <span class="search">Búsqueda:</span>
        <b-form-input
          @keyup="filters(busqueda)"
          v-model="busqueda"
          placeholder="Buscar"
          type="text"
          class="v-input-style"
        ></b-form-input>
      </b-col>
    </b-row >
    <!-- table en donde se muestran los datos al hacer el llamado a la api, css Bootstrap Vue  -->
    <b-table 
      class="table"
      :items="data"
      :responsive="true"
      sticky-header="600px"
    ></b-table>
    <!-- paginación con sus datos respectivos que se traen de la api, css Bootstrap Vue  -->
    <b-pagination
      v-model="currentPage"
      @change="nextData($event)"
      :per-page="perPage"
      :total-rows="rows"
      pills
      size="sm"
      align="center"
    ></b-pagination>
  </div>
</template>

<script>
import axios from "axios";

export default {
  name: "Home",
  data() {
    return {
      data: [],
      dataFilters: [],
      perPage: '',
      currentPage: 1,
      total: '',
      busqueda: '',
      selected: 'id',
      options: ['id', 'country','continent']
    };
  },

  mounted() {
    this.cargofiveData();
  },

  methods: {
    //Se realizó el uso de axios para poder hacer el llamado a la api y obtener los datos a utilizar
    async cargofiveData(page) {
      await axios
        .get(`http://apitest.cargofive.com/api/ports?page=${page}`)
        .then((response) => { 
          this.data = response.data.data;
          this.dataFilters = response.data.data;
          this.current_page = response.data.meta.current_page;
          this.perPage = response.data.meta.per_page;
          this.total = response.data.meta.total;
        })
        .catch((error) => {
          console.error(error);
        });
    },
    // El metodo nextData es el metodo para la paginación, donde se llama al metodo cargofiveData y asi obtener los datos siguientes
    async nextData(numberPage) {
      await this.cargofiveData(numberPage);
      //limpiar los inputs al momento de cambiar pagina 
      this.busqueda = ''
      this.selected = 'id'
    },
    // El metodo filters hace que al momento de hacer uso de los inputs pueda mostrar los datos que el usuario pida
    filters(busqueda) {
      if (busqueda.length === 0) {
        this.data = this.dataFilters;
      } else {
        //se realiza el filtro a la data y se coloca un select repecto a lo que el usuario seleccione para filtrar
        const dataFilter = this.data.filter(e => 
          e[this.selected] == busqueda
        );
        //se realiza un .length para que me traigo los datos del array data y asi se pueda realizar los filtros
        dataFilter.length > 0
          ? (this.data = dataFilter)
          : (this.data = this.dataFilters);
      }
    },
  },

  computed: {
    rows() {
      return this.total;
    },
  },
};
</script>
<style lang="scss" scoped>
.container{
  width: 100%;
  height: 100vh;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  .filters{
    width: 100%;
    margin: 30px 0;
    display: flex;
    flex-direction: column;
    .title{
      font-size: 35px;
      font-weight: bold;
    }
    .v-select-style{
      width: 100%;
      height: 38px;
      border-radius: 3px;
      border: 1px solid #D4D5D7;
      color: #878889;
      margin-bottom: 20px;
    }
    .search{
      margin-top: 10px;
    }
  }
}
@media (min-width: 768px){
  .container{
    .filters{
      width: 100%;
      margin: 30px 0;
      display: flex;
      flex-direction: row;
      .v-select-style{
        width: 100%;
        height: 38px;
        border-radius: 3px;
        border: 1px solid #D4D5D7;
        color: #878889;
      }
    }
  }
}
</style>
