<template>
    <v-card>
        <!-- Page Title -->
        <v-card-text> 
            <h1 class="text-xs-center display-3 font-weight-bold">Movie Listings</h1>    
        </v-card-text>
     
       <!-- Filter Options -->
        <v-card flat color="transparent">
            <!-- Rating slider -->
            <v-layout justify-center>
                <v-flex  shrink class="rating-slider">
                    <v-card-text class="pt-8">
                        <v-slider
                        v-model="sliderRating"
                        label="Min Rating"
                        step="0.5"
                        thumb-label="always"
                        ticks
                        min="0"
                        max="10"
                        ></v-slider>
                    </v-card-text>
                </v-flex>
            </v-layout>

            <!-- Genre select -->
            <v-layout justify-center row wrap >
                <v-flex xs8 >       
                        <v-layout row wrap >    
                            <v-flex xs6 md3 v-for="(genre, index) in filmGenres[0]" :key="index">
                                <v-checkbox :label="genre" v-model="filmGenres[1][index]" @change="removeGenre(index)"></v-checkbox>
                            </v-flex>                      
                        </v-layout>
                        <!-- Testing -->
                        <!-- <ul>
                            <li v-for="item in filmGenres[1]">{{item}}</li>
                        </ul>                       -->
                </v-flex>
            </v-layout>       
        </v-card>


        <!-- Movie Details Section -->
        <v-container >
            <v-layout  justify-center row wrap fill-height> 
                <v-flex d-flex xs12 sm6 md3 v-for="(movie, index) in moviesData" :key="index" >
                    <v-card v-if="movie.vote_average >= sliderRating" >
                        <!-- Testing -->
                        <!-- <p>{{filmGenres[0]}}</p>
                        <p>{{filmGenres[1]}}</p>
                        <p>{{movie.genre_ids}}</p> -->
                        <v-card-text class="movie-card" v-if="movie.genre_ids.length > 0"> 
                            <!-- Movie Title -->
                            <h1 class="text-xs-center movie-title headline ">{{movie.title}}</h1>      
                            <!-- Poster Image -->
                            <img :src="movie.poster_path" alt="" class="poster-image">
                            <!-- Genres -->
                            <!-- <h4 class="text-xs-center title">Genres:</h4> -->
                            <h4 class="text-xs-center subheading font-italic" v-for="(genre, index) in movie.genre_ids" :key="index">{{genre}} </h4>
                        </v-card-text>
                    </v-card>    
                </v-flex>
            </v-layout>
        </v-container>
    </v-card>
</template>

<script>
import axios from 'axios';

export default {
  data() {
    return {
      moviesData: [],
      filmGenres: {},
      sliderRating: 3,
      deletedGenres: {
          "Action": [],
          "Adventure": [],
          "Animation": [],
          "Comedy": [],
          "Crime": [],
          "Documentary": [],
          "Drama": [],
          "Family": [],
          "Fantasy": [],
          "History": [],
          "Horror": [],
          "Music": [],
          "Mystery": [],
          "Romance": [],
          "Science Fiction": [],
          "TV Movie": [],
          "Thriller": [],
          "War": [],
          "Western": [],
      }
    };
  },
  created() {
    let idMap = new Map();
    idMap.set(28, "Action");
    idMap.set(12, "Adventure");
    idMap.set(16, "Animation");
    idMap.set(35, "Comedy");
    idMap.set(80, "Crime");
    idMap.set(99, "Documentary");
    idMap.set(18, "Drama");
    idMap.set(10751, "Family");
    idMap.set(14, "Fantasy");
    idMap.set(36, "History");
    idMap.set(27, "Horror");
    idMap.set(10402, "Music");
    idMap.set(9648, "Mystery");
    idMap.set(10749, "Romance");
    idMap.set(878, "Science Fiction");
    idMap.set(10770, "TV Movie");
    idMap.set(53, "Thriller");
    idMap.set(10752, "War");
    idMap.set(37, "Western");

    axios
      .get(
        "https://api.themoviedb.org/3/movie/now_playing?api_key=4f7f99180a8a7100e411b2d21d0a90e3&language=en-US&page=1"
      )
      .then(res => {
        const moviesData = res.data.results;
        const filmGenres = [];

        for (let i = 0; i < moviesData.length; i++) {
          //concat the image path prefix to the poster paths
          moviesData[i].poster_path =
            "https://image.tmdb.org/t/p/w500" + moviesData[i].poster_path;

        //   convert the genre ids to names using the map
          let movieGenres = moviesData[i].genre_ids;
          moviesData[i].genre_ids = movieGenres.map(function(value) {
            value = idMap.get(value);
            filmGenres.push(value);
            return value;
          });
        }
        //remove duplicates from the array
        let uniqueFilmGenres = [...new Set(filmGenres)];
        let showArray = [];
        for(let i=0; i<uniqueFilmGenres.length; i++){
            showArray[i] = true;
        }

        //convert genre array and showArray to object 
        let genresObj = Object.assign({}, [uniqueFilmGenres, showArray]);
        
        
        //assign data 
        this.filmGenres = genresObj;
        this.moviesData = moviesData;
        // console.log(moviesData);
      })
      .catch(error => console.log(error));
  },
  methods: {
    removeGenre(index){
        if(!this.filmGenres[1][index]) {  
            for(let i=0; i<this.moviesData.length; i++){
                for(let j=0; j<this.moviesData[i].genre_ids.length; j++){
                    if(this.moviesData[i].genre_ids[j] === this.filmGenres[0][index]) {
                        //Store the i and j position in the deletedGenres object
                        let name = this.filmGenres[0][index]; 
                        this.deletedGenres[name].push([i,j]);
                        
                        // console.log(this.deletedGenres)
                        

                        let el = this.moviesData[i].genre_ids.indexOf(this.filmGenres[0][index])
                        if(el > -1){
                            this.moviesData[i].genre_ids.splice(el, 1);
                        }
                    }
                }
            }
        }else{
            let name = this.filmGenres[0][index];
            // console.log(name);
                for(let i=0; i< this.deletedGenres[name].length; i++){
                    let iPos = this.deletedGenres[name][i][0];
                    let jPos = this.deletedGenres[name][i][1];
                    //Add the genres back to their positions in genre_ids 
                    this.moviesData[iPos].genre_ids.splice(jPos, 0 , name);                               
                }
                //Remove the restored positions from the deletedGeneres Object
                this.deletedGenres[name] = [];         
            
            
            // console.log(this.deletedGenres)
        }         
    },
  }
};
</script>

<style>
    .movie-title{
        height: 70px;
    }
    .poster-image{
        max-width: 100%;
    }
    .movie-card{
        margin-bottom: 50px;
    }
    .rating-slider{
        width: 60%;
    }
</style>

