<template>
<div id="pokemon">
    <div class="card">
        <div class="card-image">
            <figure>
                <img :src="pokemon.currentImg" alt="Placeholder image">
            </figure>
        </div>
        <div class="card-content">
            <div class="media">
                <div class="media-content">
                
                    <p class="title is-4">{{ upper(name) }}</p>
                    <p class="subtitle is-6">{{ pokemon.type }}</p>
                </div>
            </div>
            <button class="button is-link" @click="pokemon.isFront = !pokemon.isFront; pokemon.currentImg = pokemon.isFront ? pokemon.front : pokemon.back">Flip</button>
        </div>
    </div>
</div>
        

</template>

<script>
import axios from 'axios';


export default {

    props:{
        num: Number,
        name: String,
        url: String
    },

    data(){
        return{
            pokemon:{
                isFront: true,
                currentImg: '',
                type: '',
                front: '',
                back: ''
            }
        }
    },
    
    created: function() {
        axios.get(this.url)
        .then(response => {
            this.pokemon.type = response.data.types[0].type.name;
            this.pokemon.front  = response.data.sprites.front_default;
            this.pokemon.currentImg = this.pokemon.front;
            this.pokemon.back  = response.data.sprites.back_default;
            console.log(this.pokemon);
        })
        .catch(function(error) {
            console.log(error);
        });
    },
    
    methods: {
        upper(value) {
            return value.charAt(0).toUpperCase() + value.slice(1);
        }
    }
}
</script>

<style>
#pokemon{
    margin:2%;
}
</style>