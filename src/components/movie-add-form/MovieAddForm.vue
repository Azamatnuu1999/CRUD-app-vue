<template>
    <div class="movie-add-form">
       <h3>Yangi kino qo'shish</h3>
       <form class="add-form d-flex" @submit.prevent>
        <Input 
        class="new-movie-label" 
        placeholder="Qanday kino?"
        v-model="name"
        />
        <Input
         class="new-movie-label"
         placeholder="Nechi marotaba ko'rilgan?" 
         type="number"
         v-model="viewers"
        />
        <PrimaryButton
        class="btn-outline-dark" type="submit" @click="addMovie">Qo'shish</PrimaryButton>
       </form>
    </div>
</template>

<script>
export default {
    data(){
        return{
            name: '',
            viewers: '',
        }
    },
    methods:{
        addMovie(){
            if(!this.name || !this.viewers) return 
            const newMovie = {
                name: this.name,
                viewers: this.viewers,
                favourite: false,
                like: false,
                id: Date.now()
            }
            this.$emit('createMovie',newMovie)
            this.name = ''
            this.viewers = ''
        }
    }
}
</script>

<style scoped>
    .movie-add-form{
        padding: 1.5rem;
        margin-top: 2rem;
        background-color: #fcfaf5;
        border-radius: 4px;
        box-shadow: 15px 15px 15px rgba(0,0,0,0.15);
    }
</style>