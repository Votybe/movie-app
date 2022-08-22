<template>
  <div class="home">
    <div class="filter__container">
      <div class="filter-gender">
        <h2>Search a movie by gender</h2>
        <ul class="genderList__container">
          <li @click="getMovieByGender()">all</li>
          <li
            v-for="(genderItem, index) in gender"
            :key="index"
            @click="getMovieByGender(genderItem)"
          >
            {{ genderItem.name }}
          </li>
        </ul>
      </div>
      <h2 class="title__advancedfilter">Advanced filter</h2>

      <div class="second-category">
        <div class="filter-name">
          <font-awesome-icon class="icon" icon="fa-solid fa-magnifying-glass" />
          <input
            type="text"
            placeholder="search by name"
            v-model="movieField"
          />
        </div>

        <div class="advanced-filter">
          <select name="" id="" value="ezfz">
            <option value="" selected disabled hidden>Sort by</option>
            <option value="average">average</option>
            <option value="year">year</option>
            <option value="name">name</option>
          </select>
        </div>
        <div class="filter-page">
          <button :disabled="currentPage === 1" @click="prevPage">Prev</button>
          <p>page {{ currentPage }}</p>
          <button @click="nextPage">Next</button>
        </div>
      </div>
    </div>

    <h2>All movies</h2>

    <ul class="movieList__container">
      <li v-for="(movie, index) in movies" :key="index">
        <img
          :src="
            'https://www.themoviedb.org/t/p/w220_and_h330_face/' +
            movie.backdrop_path
          "
          :alt="movie.original_title"
        />
        <p id="title">{{ movie.title }}</p>
        <div class="container__data">
          <p>{{ movie.release_date }}</p>
          <p id="average">{{ movie.vote_average }}</p>
        </div>
      </li>
    </ul>
  </div>
</template>

<script>
import axios from "axios";

export default {
  name: "HomeView",
  components: {},
  data() {
    return {
      test: "Bienvenu dans la page web",
      gender: [],
      movies: [],
      genderActif: -1,
      currentPage: 1,
      movieField: "",
    };
  },
  watch: {
    movieField: function () {
      this.search();
    },
  },

  methods: {
    async search() {
      if (this.movieField == "") {
        this.getItems();
      } else {
        await axios
          .get(
            "https://api.themoviedb.org/3/search/movie?api_key=6dc646632d1c11debbc7e874ea32f797&query=" +
              this.movieField
          )
          .then((response) => {
            console.log(response.data.results[0]);
            this.movies = response.data.results;
            for (let index = 0; index < this.movies.length; index++) {
              if (this.movies[index].backdrop_path == null) {
                this.movies.splice(index);
              }
            }
          });
      }
    },

    async prevPage() {
      if (this.currentPage !== 1) {
        let result = await axios.get(
          `https://api.themoviedb.org/3/list/${this
            .currentPage--}?api_key=6dc646632d1c11debbc7e874ea32f797`
        );
        this.movies = result.data.items;
      }
    },
    async nextPage() {
      let result = await axios.get(
        `https://api.themoviedb.org/3/list/${this
          .currentPage++}?api_key=6dc646632d1c11debbc7e874ea32f797`
      );
      this.movies = result.data.items;
      console.log(this.movies);
    },
    async getMovieByGender(genderItem) {
      console.log(this.currentPage);
      if (this.genderActif === -1) {
        let result = await axios.get(
          `https://api.themoviedb.org/3/list/${
            this.currentPage - 1
          }?api_key=6dc646632d1c11debbc7e874ea32f797`
        );
        this.movies = result.data.items;
      }

      let movTemp = [];

      for (const iterator of this.movies) {
        if (iterator.genre_ids.includes(genderItem.id)) {
          movTemp.push(iterator);
        }
      }

      this.movies = movTemp;
    },
    async getGenres() {
      let genders = await axios.get(
        "https://api.themoviedb.org/3/genre/movie/list?api_key=6dc646632d1c11debbc7e874ea32f797"
      );

      this.gender = genders.data.genres;
    },
  },
  mounted() {
    this.getGenres();
    this.getMovieByGender();
  },
};
</script>

<style lang="scss" scoped>
.home {
  background-color: #202427;
  min-height: 100vh;
  min-width: 100%;
  display: flex;
  flex-direction: column;
  align-items: center;
  color: #fff;

  .filter__container {
    display: flex;
    flex-direction: column;
    align-items: center;
    .title__advancedfilter {
      margin: 1rem 0;
    }
    .filter-gender {
      display: flex;
      flex-direction: column;
      width: 60%;

      h2 {
        margin: 1rem auto;
      }
      .genderList__container {
        align-items: center;
        display: flex;
        flex-wrap: wrap;
        margin: 0 auto;
        li {
          list-style-type: none;
          border-radius: 9px;
          color: #b5e4ca;
          margin: 0.3rem;
          border: solid 1px #b5e4ca;
          background-color: #2b3034;
          padding: 0.5rem;
          font-size: 14px;

          &:hover {
            background-color: #b5e4ca;
            cursor: pointer;
            color: #000;
          }
        }
      }
    }

    .second-category {
      display: flex;
      align-items: center;
      justify-content: center;
      margin: 0.3rem;
      .filter-name {
        display: flex;
        margin: 0.4rem;
        border: solid 1px #b5e4ca;
        padding-left: 0.3rem;
        align-items: center;

        color: #b5e4ca;
        .icon {
          margin: 0.3rem;
          color: #b5e4ca;
        }
        input {
          border: none;
          background: none;
          border-radius: 9px;
          height: 2rem;
          width: 14rem;
          outline: none;
          &::placeholder {
            color: #b5e4ca;
          }
        }
      }

      .advanced-filter {
        margin: 0 4rem;
        width: 30%;
        display: flex;
        flex-direction: column;
        align-items: center;
        select {
          background-color: #202427;
          color: #b5e4ca;
          border: solid 1px #b5e4ca;
          padding: 0.3rem 0.4rem;
          outline: none;
          border-radius: 9px;
        }
      }

      .filter-page {
        padding: 0.4rem;
        display: flex;
        align-items: center;
        width: 100%;
        p {
          padding: 0 1rem;
        }
        button {
          background-color: #b5e4ca;
          color: #202427;
          padding: 0.7rem 1.2rem;
          border-radius: 9px;
          margin: 0.4rem;
          border: none;

          &:hover {
            cursor: pointer;
          }
        }
      }
    }
  }

  h2 {
    display: flex;
    font-size: 1.1rem;
  }

  .movieList__container {
    display: flex;
    justify-content: center;
    flex-wrap: wrap;
    width: 100%;
    li {
      display: flex;
      flex-direction: column;
      background-color: #2b3034;
      box-shadow: rgba(0, 0, 0, 0.24) 0px 3px 8px;
      color: #b5e4ca;
      width: 260px;
      margin: 0.4rem;
      list-style-type: none;
      height: 350px;

      #title {
        font-size: 18px;
        height: 3rem;
        text-align: center;
        padding: 0.6rem 0;
      }

      .container__data {
        display: flex;
        height: 100%;
        display: flex;
        justify-content: space-between;
        padding: 0 0.4rem;
        align-items: center;
        margin: 0.3rem;

        #average {
          background-color: #b5e4ca;
          border-radius: 50%;
          color: #2b3034;
          padding: 0.4rem;
        }
      }
      &:hover {
        cursor: pointer;
        transform: scale(1.04);
      }

      img {
        height: 250px;
      }
    }
  }
}
</style>
