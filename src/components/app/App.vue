<template>
  <div class="app font-monospace">
    <div class="content">
      <AppInfo :allMoviesCount="movies.length" :favouriteMoviesCount="movies.filter(c => c.favourite).length" />
      <div class="search-panel">
        <SearchPanel :updateTermHandler="updateTermHandler"
         />
        <AppFilter :updateFilterHandler="updateFilterHandler"
        :filterName="filter"
         />
      </div>
      <Box v-if="!movies.length && !isLoading">
        <p class="text-center fs-3">Kinolar yo'q</p>
      </Box>
      <Box v-if="isLoading" class="d-flex justify-content-center">
        <Loader/>
      </Box>
      <MovieList 
      v-else
      :movies="onFilterHandler(onSearchHandler(movies, term), filter)" @onToggle="onToggleHandler" 
      />
      <Box class="d-flex justify-content-center">
        <nav aria-label="...">
        <ul class="pagination pagination-lg">
          <li 
          v-for="pageNumber in totalPages" :key="pageNumber" 
          :class="{'active' 
          : pageNumber == page}"
          @click="changePageHandler(pageNumber)"
          >
            <span class="page-link">
              {{ pageNumber }}
            </span>
          </li>
        </ul>
      </nav>
      </Box>
      <MovieAddForm @createMovie="createMovie" />
    </div>
  </div>
</template>

<script>
import AppInfo from '@/components/app-info/AppInfo.vue'
import SearchPanel from '../search-panel/SearchPanel.vue'
import MovieList from '../movie-list/MovieList.vue'
import MovieAddForm from '../movie-add-form/MovieAddForm.vue'
import AppFilter from '../app-filter/AppFilter.vue'
import axios from 'axios'
import Box from '../ui-components/Box.vue'
export default {
  components:{
    AppInfo,
    SearchPanel,
    MovieList,
    MovieAddForm,
    AppFilter,
    Box
},
  data(){
    return {
      movies:[],
      term: '',
      filter: 'all',
      isLoading: false,
      limit: 10,
      page: 1,
      totalPages: 0
    }
  },
  methods:{
    createMovie(item){
      this.movies.push(item)
    },
    onToggleHandler({id,prop}){
      if(prop == 'trash'){
        this.movies = this.movies.filter((el) => {
          return el.id !== id
        })
      }else{
        this.movies = this.movies.map(item => {
        if(item.id == id){
          item[prop] = !item[prop]
        }
        return item
      })
      }
    },
    onSearchHandler(arr, term){
      if(term.length == 0) return arr
      return arr.filter(el => el.name.toLowerCase().indexOf(term) > -1)
    },
    onFilterHandler(arr, filter){
      switch(filter){
        case 'popular':
          return arr.filter(c => c.like)
        case 'mostViewers':
          return arr.filter(c => c.viewers > 50)
        default:
          return arr
      }
    },
    updateTermHandler(term){
      this.term = term
    },
    updateFilterHandler(filter){
      this.filter = filter
    },
    changePageHandler(page){
      this.page = page
    },
    async fetchMovie(){
      this.isLoading = true
        try {
          const response = await axios.get('https://jsonplaceholder.typicode.com/posts', {
            params:{
              _page: this.page,
              _limit: this.limit
            }
          })
          const newArr = response.data.map(item => ({
            id: item.id,
            name: item.title,
            like: false,
            favourite: false,
            viewers: item.id * 10
          }))
          this.totalPages = Math.ceil(response.headers['x-total-count'] / this.limit)
          this.movies = newArr
        } catch (error) {
          console.log(error.message)
        } finally {
          this.isLoading = false
        }
    }
  },
  mounted(){
    this.fetchMovie()
  },
  watch:{
    page(){
      this.fetchMovie()
    }
  }
}
</script>

<style>
  .app{
    height: 100vh;
    color: #000;
  }
  .content{
    width: 1000px;
    min-height: 700px;
    background-color: white;
    margin: 0 auto;
    padding: 5rem 0;
  }
  .search-panel{
    padding: 1.5rem;
    margin-top: 2rem;
    background-color: #fcfaf5;
    border-radius: 4px;
    box-shadow: 15px 15px 15px rgba(0,0,0,0.15);
  }
</style>