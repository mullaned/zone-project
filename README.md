# Movie Listings Project

Built using the Vue framework and Vuetify UI library.  Axios is used to request the now playing listings from the TMDb API.  The required info is the movie title, genres and poster image for each movie.  On creation axios makes the request, data object is received in order of popularity, the poster image path is concated with the url prefix and the genre ids are converted to their named values.  The required info is passed to the component data object.

##Filters
###The Slider filter:
The slider filter has a range of 0 - 10 in 0.5 step increments.  Any movies with a `vote_average` property of less than the min rating will be removed from the list using a v-if statement.

###The Genre filter:
The data returned from the get request provides the genres in number format.  Using a map function the numbers are converted to their names and placed in an array.  Duplicates are removed from this array and a second array of true values is built matching the length of the unique genres array.

The unique genres array and the true values array are combined into a object and data is set on creation.

This object `filmGenres` has `filmGenres[0] = names` and `filmGenres[1] = true value` as all checkbox will be ticked on creation.
V-for is used to create the checkboxes on `filmGenres[0]` displaying only the genres that are present in the movies returned from the get request.
The checkboxs are binded to `filmGenres[1]` with a initial value of true as all listed genres are listed if the have a `vote_average` rating >= 3. 



##Styling
Basic styling for the page, Vuetify fonts, checkboxes, slider and cards for the movies info.
The page is responsive using flexbox.





[aws-url] : http://movie-listings.s3-website-us-east-1.amazonaws.com/;

## Project setup
```
npm install
```

### Compiles and hot-reloads for development
```
npm run serve
```

### Compiles and minifies for production
```
npm run build
```


### Lints and fixes files
```
npm run lint
```
