<template>
    <div>
         
        <div class="bg-img" :style="{'background-image': 'url ('+backdropUrl+')'}">

        <v-ons-row class="movie-top">
            <!--<v-ons-checkbox
                    :input-id="'checkbox-' + $index"
                    :value="color"
                    v-model="checkedColors"
                ><youtube v-if="videoId" :video-id="videoId"></youtube> -->
        </v-ons-row>
        <v-ons-row id="movie-main"> <!-- General movie info -->
            <v-ons-col width="45.5vw">
                <div class="poster-card"><img class="poster-big" :src="imageUrl"></img></div>
            </v-ons-col>
            <v-ons-col style="marginLeft:4vw;">
                <h1 class="title">{{ movie.title }}</h1>
                <span class="year">{{ formattedDate }}</span>
                <v-ons-row class="movie-score">
                    <ons-icon icon="fa-star" size="9vw"></ons-icon>
                    <span class="main-score">{{ movie.vote_average }}</span>
                </v-ons-row>
            </v-ons-col>
        </v-ons-row>

        </div>

        <v-ons-row id="interactables">
            <v-ons-col>
                <v-ons-button @click="toggleWatched()" modifier="quiet">Watched</v-ons-button>
            </v-ons-col>
            <v-ons-col>
                <v-ons-button @click="toggleToWatch()" modifier="quiet">To-Watch</v-ons-button>
            </v-ons-col>
            <v-ons-col>
                <v-ons-button @click="toggleFavorite()" v-show="this.liked"
                modifier="quiet">Favorite</v-ons-button>
            </v-ons-col>
            <!-- <v-ons-button><v-ons-col>
                <v-ons-checkbox modifier="material" ></v-ons-checkbox><br>
                <label>Add to List</label>
            </v-ons-col></v-ons-button>
           <v-ons-col>
                <v-ons-button><ons-icon size="10vw" icon="ion-thumbsup"></ons-icon></v-ons-button>
                <v-ons-button><ons-icon size="11vw" icon="ion-thumbsdown"></ons-icon><br></v-ons-button>
                <label style="margin-right: 7vw;">Like</label>
                <label>Dislike</label>
            </v-ons-col>
            <v-ons-button><v-ons-col>
                <ons-icon size="8vw" icon="heart"></ons-icon><br>
                <label>Favorite</label>
            </v-ons-col></v-ons-button>  -->
        </v-ons-row>
        <v-ons-fab position="bottom right" :visible="true">
            <v-ons-icon icon="md-face"></v-ons-icon>
        </v-ons-fab>
    </div>
</template>

<script>
export default {
  data() {
    return {
      videoId: null,
      monthNames: [
        "Jan", "Feb", "Mar", "Apr", "May", "Jun", 
        "Jul", "Aug", "Sep", "Oct", "Nov", "Dec"
      ],
      date: null,
      watchedMovies: this.$localStorage.get('watchedMovies'),
      toWatchMovies: this.$localStorage.get('toWatchMovies'),
      liked: true,
      toWatched: false,
      favorited: false
    }
  },
  created() {
    this.date = new Date(this.movie.release_date);
    // note to self: get liked, to watched, favorited
  }, 
  computed: {
    imageUrl: function() {
      return "http://image.tmdb.org/t/p/w185/"+this.movie.poster_path;
    },
    formattedDate: function() {
        return this.monthNames[this.date.getMonth()]+" "+this.date.getDate()+
        " "+this.date.getFullYear();
    },
    backdropUrl: function() {
        return "http://image.tmdb.org/t/p/w185"+this.movie.backdrop_path;
    },
    watchedContains: function() {
        return this.watchedMovies.includes(this.movie);
    },
    toWatchContains: function() {
        return this.toWatchMovies.includes(this.movie);
    }
  },
  methods: {
    toggleWatched() {
        console.log(this.watchedMovies);
        if (!this.watchedContains) { // Check whether the movie is already added
            if (this.watchedMovies === undefined) {
                this.watchedMovies = [];    // Initialize array just in case
            }
            this.watchedMovies.push(this.movie);
            this.$localStorage.set('watchedMovies', this.watchedMovies);
            this.$ons.notification.toast("Added  "+this.movie.title+" to My Movies", {timeout:3000});
        }
        else {
            const index = this.watchedMovies.indexOf(this.movie);
            this.watchedMovies.splice(index, 1);
            this.$ons.notification.toast("Removed  "+this.movie.title+" from My Movies", {timeout:3000});
        }
        
    },
    toggleToWatch() {
        if (!this.watchedContains) { // Check whether the movie is already added
            if (this.toWatchMovies === undefined) {
                this.toWatchMovies = [];
            }
            this.toWatchMovies.push(this.movie);
            this.$localStorage.set('toWatchMovies', this.toWatchMovies);
            this.$ons.notification.toast("Added "+this.movie.title+" to To-Watch", {timeout:3000});
        }
        else {
            const index = this.toWatchMovies.indexOf(this.movie);
            this.toWatchMovies.splice(index, 1);
            this.$ons.notification.toast("Removed  "+this.movie.title+" from To-Watch", {timeout:3000});
        }
    },
    toggleFavorite() {
        let movies = this.$localStorage.get('watchedMovies');
        movies.filter(function(movie) {
            if (movie.id == this.movie.id) {
                if (movie.favorite) {
                    movie.favorite = undefined;
                    this.$ons.notification.toast("Removed "+this.movie.title+" from Favorites", {timeout:3000});
                }
                else {
                    movie.favorite = true;
                    console.log(movie.favorite);
                    this.$ons.notification.toast("Added "+this.movie.title+" to Favorites", {timeout:3000});
                }
            }
        }.bind(this));
        this.$localStorage.set('watchedMovies', movies);
    },
    /* openFab() {
        $ons.openActionSheet({buttons:['label1', 'label2', 'label3'], 
        title: 'Lorem ipsum', cancelable: true, destructive: 1});
    } */
  },
  props: ['movie'],
  components: {  }
}
</script>
