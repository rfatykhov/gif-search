<template>
  <div class="container-fluid">
    <nav class="navbar navbar-expand-lg navbar-light bg-light">
      <button
        class="navbar-toggler"
        type="button"
        data-toggle="collapse"
        data-target="#navbarNavAltMarkup"
        aria-controls="navbarNavAltMarkup"
        aria-expanded="false"
        aria-label="Toggle navigation"
      >
        <span class="navbar-toggler-icon"></span>
      </button>
      <div class="collapse navbar-collapse" id="navbarNavAltMarkup">
        <div class="navbar-nav">
          <nuxt-link
            exact
            no-prefetch
            active-class="active"
            class="nav-item nav-link active"
            to="/"
          >
            Домой
          </nuxt-link>
          <nuxt-link
            active-class="active"
            class="nav-item nav-link"
            to="/about"
          >
            О сайте</nuxt-link
          >
          <b-button v-b-modal.modal-1 class="btn-danger float-right"
            >Избранное</b-button
          >
          <b-modal id="modal-1" title="Избранные гифки">
            <p v-if="favorites.length === 0">
              Вы пока не добавили ничего в избранное
            </p>
            <div v-for="data in favorites" :key="data.id">
              <div>
                <div class="card">
                  <video
                    class="modal-video"
                    :key="data.id"
                    type="video"
                    :src="data.images.original.mp4"
                    autoplay=""
                    loop=""
                  ></video>
                  <b-button @click.prevent="remove(data.id)"
                    ><b-icon-trash></b-icon-trash> Удалить
                  </b-button>
                </div>
              </div>
            </div>
          </b-modal>
        </div>
      </div>
    </nav>
    <div></div>
    <form class="form-inline">
      <input
        class="form-control mx-auto w-75 p-3"
        type="search"
        placeholder="Search"
        aria-label="Search"
        v-model="query"
      />
      <button class="btn btn-success" type="submit" @click.prevent="search()">
        Искать гифки
      </button>
    </form>

    <div v-for="data in result" :key="data.id" class="gallery">
      <div class="element">
        <div class="card">
          <video
            class="video"
            :key="data.id"
            type="video"
            :src="data.images.original.mp4"
            autoplay=""
            loop=""
          ></video>
          <b-button @click.prevent="add(data.id)" class="btn-danger"
            ><b-icon-star></b-icon-star> В избранное
          </b-button>
        </div>
      </div>
    </div>

    <div v-for="data in trend" :key="data.id" class="gallery">
      <div class="element">
        <div class="card">
          <video
            class="video"
            :key="data.id"
            type="video"
            :src="data.images.original.mp4"
            autoplay=""
            loop=""
          ></video>
          <b-button @click.prevent="add(data.id)" class="btn-danger"
            ><b-icon-star></b-icon-star> В избранное
          </b-button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      trend: [],
      result: [],
      query: "",
      favorites: []
    };
  },
  async fetch() {
    await fetch(
      "https://api.giphy.com/v1/gifs/trending?api_key=39vI8skUqOzHnLMOJZiRkLKJ7SIWbQ4J&q=&limit=10"
    )
      .then(response => {
        return response.json();
      })
      .then(response => {
        this.trend = response.data;
      })
      .catch(error => console.log(error));
  },

  methods: {
    search() {
      fetch(
        "https://api.giphy.com/v1/gifs/search?api_key=39vI8skUqOzHnLMOJZiRkLKJ7SIWbQ4J&q=" +
          this.query +
          "&limit=10"
      )
        .then(response => {
          return response.json();
        })
        .then(response => {
          this.result = response.data;
        })
        .catch(error => console.log(error));
    },
    add(id) {
      fetch(
        "https://api.giphy.com/v1/gifs/" +
          id +
          "?api_key=39vI8skUqOzHnLMOJZiRkLKJ7SIWbQ4J&q"
      )
        .then(response => {
          return response.json();
        })
        .then(response => {
          this.favorites.push(response.data);
        })
        .catch(error => console.log(error));
    },
    remove(id) {
      this.favorites = this.favorites.filter(el => el.id !== id);
      console.log(this.favorites);
    }
  }
};
</script>

<style>
body {
  background-color: black;
}

.form-inline {
  margin: 15px auto;
}

.gallery {
  width: 100%;
  min-height: 100;
  margin: 0 auto;
  text-align: center;
}

.element {
  float: left;
}

.video {
  height: 500px;
  width: 500px;
  padding: 1rem;
}

.card {
  display: flex;
  flex-direction: column;
  background-color: black;
}

.modal-video {
  width: 100%;
}
</style>
