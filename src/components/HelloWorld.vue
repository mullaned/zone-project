<template>
  <div id="now-playing-list">
    <div v-for="genre in filmGenres">
      <p>{{genre}}</p>
    </div>
    <div v-for="movie in moviesData">
      <h3>{{movie.title}}</h3>
      <img class="poster-image" :src="movie.poster_path" alt="">
      
      <h4 v-for="genre in movie.genre_ids">{{genre}}</h4>
    </div>
  </div>
</template>


<script>
import axios from "axios";

export default {
  name: "search",
  props: {
    msg: String
  },
  data() {
    return {
      moviesData: [],
      filmGenres: []
    };
  },
  created() {
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

          //translate the genre id to text and add the listed genres to the filmGenres array
          for (let j = 0; j < moviesData[i].genre_ids.length; j++) {
            if (moviesData[i].genre_ids[j] === 28) {
              moviesData[i].genre_ids[j] = "Action";
              filmGenres.push(moviesData[i].genre_ids[j]);
            } else if (moviesData[i].genre_ids[j] === 12) {
              moviesData[i].genre_ids[j] = "Adventure";
              filmGenres.push(moviesData[i].genre_ids[j]);
            } else if (moviesData[i].genre_ids[j] === 16) {
              moviesData[i].genre_ids[j] = "Animation";
              filmGenres.push(moviesData[i].genre_ids[j]);
            } else if (moviesData[i].genre_ids[j] === 35) {
              moviesData[i].genre_ids[j] = "Comedy";
              filmGenres.push(moviesData[i].genre_ids[j]);
            } else if (moviesData[i].genre_ids[j] === 80) {
              moviesData[i].genre_ids[j] = "Crime";
              filmGenres.push(moviesData[i].genre_ids[j]);
            } else if (moviesData[i].genre_ids[j] === 99) {
              moviesData[i].genre_ids[j] = "Documentary";
              filmGenres.push(moviesData[i].genre_ids[j]);
            } else if (moviesData[i].genre_ids[j] === 18) {
              moviesData[i].genre_ids[j] = "Drama";
              filmGenres.push(moviesData[i].genre_ids[j]);
            } else if (moviesData[i].genre_ids[j] === 10751) {
              moviesData[i].genre_ids[j] = "Family";
              filmGenres.push(moviesData[i].genre_ids[j]);
            } else if (moviesData[i].genre_ids[j] === 14) {
              moviesData[i].genre_ids[j] = "Fantasy";
              filmGenres.push(moviesData[i].genre_ids[j]);
            } else if (moviesData[i].genre_ids[j] === 36) {
              moviesData[i].genre_ids[j] = "History";
              filmGenres.push(moviesData[i].genre_ids[j]);
            } else if (moviesData[i].genre_ids[j] === 27) {
              moviesData[i].genre_ids[j] = "Horror";
              filmGenres.push(moviesData[i].genre_ids[j]);
            } else if (moviesData[i].genre_ids[j] === 10402) {
              moviesData[i].genre_ids[j] = "Music";
              filmGenres.push(moviesData[i].genre_ids[j]);
            } else if (moviesData[i].genre_ids[j] === 9648) {
              moviesData[i].genre_ids[j] = "Mystery";
              filmGenres.push(moviesData[i].genre_ids[j]);
            } else if (moviesData[i].genre_ids[j] === 10749) {
              moviesData[i].genre_ids[j] = "Romance";
              filmGenres.push(moviesData[i].genre_ids[j]);
            } else if (moviesData[i].genre_ids[j] === 878) {
              moviesData[i].genre_ids[j] = "Science Fiction";
              filmGenres.push(moviesData[i].genre_ids[j]);
            } else if (moviesData[i].genre_ids[j] === 10770) {
              moviesData[i].genre_ids[j] = "TV Movie";
              filmGenres.push(moviesData[i].genre_ids[j]);
            } else if (moviesData[i].genre_ids[j] === 53) {
              moviesData[i].genre_ids[j] = "Thriller";
              filmGenres.push(moviesData[i].genre_ids[j]);
            } else if (moviesData[i].genre_ids[j] === 10752) {
              moviesData[i].genre_ids[j] = "War";
              filmGenres.push(moviesData[i].genre_ids[j]);
            } else if (moviesData[i].genre_ids[j] === 37) {
              moviesData[i].genre_ids[j] = "Western";
              filmGenres.push(moviesData[i].genre_ids[j]);
            }
          }
        }
        //remove duplicates from the array
        let uniqueFilmGenres = [...new Set(filmGenres)];

        this.filmGenres = uniqueFilmGenres;
        this.moviesData = moviesData;
        console.log(moviesData);
      })
      .catch(error => console.log(error));
  },

  methods: {
    
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
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
  color: #42b983;
}

.poster-image {
}
</style>
