<template>
  <div class="movie">
    <div class="container__movie">
      <div class="movie__title">
        <h2>{{ movie.title }}</h2>
        <span>{{ movie.tagline }}</span>
      </div>
      <div class="movieContainer__body">
        <img
          class="movie-image"
          :src="
            'https://www.themoviedb.org/t/p/w220_and_h330_face/' +
            movie.backdrop_path
          "
          :alt="movie.original_title"
        />
        <div>
          <div class="menu">
            <ul>
              <li @click="showDescriptionModal">Description</li>
              <li @click="showInformationModal">Informations</li>
              <li @click="showCastingModal">Casting</li>
              <li @click="showCompaniesModal">Companies</li>
              <li @click="showTrailersModal">Trailers</li>
            </ul>
          </div>
          <div v-if="showDescription" class="container-description">
            <ul>
              <li v-for="(element, index) in movie.genres" :key="index">
                {{ element.name }}
              </li>
            </ul>
            <p class="overview">{{ movie.overview }}</p>
            <p class="website-link">
              <a :href="movie.homepage">Site Web : www.{{ movie.title }}.com</a>
            </p>
          </div>
          <div v-if="showInformation">
            <ul>
              <li>status : {{ movie.status }}</li>
              <li>original title : {{ movie.original_title }}</li>
              <li>original language : {{ movie.original_language }}</li>
              <li>release date : {{ movie.release_date }}</li>
              <li>Budget : {{ movie.budget }}</li>
              <li>Runtime : {{ movie.runtime }}</li>
              <li>Revenue : {{ movie.revenue }}</li>
              <li>Popularity : {{ movie.popularity }}</li>
              <li>vote average : {{ movie.vote_average }}</li>
              <li>vote count : {{ movie.vote_count }}</li>
            </ul>
          </div>
          <div v-if="showCasting" class="container-actors">
            <ul>
              <li v-for="(actor, index) in actors" :key="index">
                <p>{{ actor.original_name }}</p>
                <p>{{ actor.character }}</p>
                <img
                  v-if="actor.profile_path !== ''"
                  :src="
                    'https://www.themoviedb.org/t/p/w220_and_h330_face/' +
                    actor.profile_path
                  "
                  :alt="actor.original_name"
                />
                <p>{{ actor.popularity }}</p>
              </li>
            </ul>
          </div>
          <div v-if="showCompanies">je suis dans companies</div>
          <div v-if="showTrailers">je suis dans trailers</div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";
export default {
  name: "MovieView",
  components: {},
  data() {
    return {
      actors: [],
      movie: {},
      cocher: false,
      showInformation: false,
      showDescription: false,
      showCompanies: false,
      showCasting: true,
      showTrailers: false,
    };
  },

  methods: {
    showDescriptionModal() {
      this.showDescription = true;
      this.showInformation = false;
      this.showCasting = false;
      this.showCompanies = false;
      this.showTrailers = false;
    },
    showInformationModal() {
      this.showInformation = true;
      this.showDescription = false;
      this.showCasting = false;
      this.showCompanies = false;
      this.showTrailers = false;
    },
    showCastingModal() {
      this.showInformation = false;
      this.showDescription = false;
      this.showCasting = true;
      this.showCompanies = false;
      this.showTrailers = false;
    },
    showCompaniesModal() {
      this.showInformation = false;
      this.showCasting = false;
      this.showDescription = false;
      this.showCompanies = true;
      this.showTrailers = false;
    },
    showTrailersModal() {
      this.showInformation = false;
      this.showCasting = false;
      this.showDescription = false;
      this.showCompanies = false;
      this.showTrailers = true;
    },
    async getActors() {
      let result = await axios.get(
        `https://api.themoviedb.org/3/movie/${this.$route.params.movieid}/credits?api_key=6dc646632d1c11debbc7e874ea32f797&language=en-US`
      );
      this.actors = result.data.cast;
      console.log(result.data);
    },
  },

  async mounted() {
    let result = await axios.get(
      "https://api.themoviedb.org/3/movie/" +
        this.$route.params.movieid +
        "?api_key=6dc646632d1c11debbc7e874ea32f797"
    );
    this.movie = result.data;
    console.log(this.movie);
    this.getActors();
  },
};
</script>

<style lang="scss" scoped>
.movie {
  background-color: #202427;
  min-height: 89.2vh;
  display: flex;
  flex-direction: column;
  align-items: flex-start;
  color: #fff;
  width: 100%;
  .cocher {
    border-bottom: solid 1px blue;
  }

  .container__movie {
    display: flex;
    margin: auto;
    height: 80vh;
    background-color: #2b3034;
    width: 95%;
    flex-direction: column;

    .movie__title {
      margin: 1.5rem auto;
      display: flex;
      flex-direction: column;
      align-items: center;
    }

    span {
      font-style: italic;
      margin: 0.4rem;
    }

    .movieContainer__body {
      display: flex;
      .movie-image {
        margin: auto 4rem;
        max-width: 400px;
        height: 450px;
      }

      .container-description {
        margin: 0.4rem;
        ul {
          display: flex;
          li {
            list-style-type: none;
            margin: 2rem 0.4rem;
          }
        }

        .overview {
          width: 65%;
        }

        .website-link {
          margin: 2rem 0;
          a {
            text-decoration: none;
            color: white;
          }
        }
      }

      .container-actors {
        scroll-behavior: auto;
        max-height: 80%;

        overflow: scroll;
        ul {
          display: flex;
          flex-wrap: wrap;
          li {
            width: 15%;
            margin: 0.3rem 2rem;
            display: flex;
            flex-direction: column;
            align-items: center;
            list-style: none;
            img {
              width: 130px;
              height: 130px;
              border-radius: 50%;
            }
          }
        }
      }

      .menu {
        display: flex;
        align-items: center;
        ul {
          background-color: #13171a;
          display: flex;
          min-width: 60%;
          li {
            height: 3rem;
            list-style: none;
            display: flex;
            align-items: center;
            margin: 1rem;
            padding: 0.2rem;
            border: none;

            &:hover {
              background: #2b3034;
              cursor: pointer;
            }
          }
        }
      }
    }
  }
}
</style>
