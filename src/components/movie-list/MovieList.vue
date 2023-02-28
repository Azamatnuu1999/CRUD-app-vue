<template>
    <ul class="movie-list list-group">
        <MovieListItem v-for="movie in movies" v-bind:movie="movie" :key="movie.id" @onToggle="$emit('onToggle', $event)"
        />
    </ul>
</template>

<script>
import MovieListItem from '../movie-list-item/MovieListItem.vue';
export default {
    components:{
        MovieListItem
    },
    props:{
        movies:{
            type: Object,
            required: true,
        }
    },
}
</script>

<style scoped>
    .movie-list{
        padding: 1.5rem;
        margin-top: 2rem;
        background-color: #fcfaf5;
        border-radius: 4px;
        box-shadow: 15px 15px 15px rgba(0,0,0,0.15);
    }
</style>