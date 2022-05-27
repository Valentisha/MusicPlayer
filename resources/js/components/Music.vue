<template>
  <div class="page__center wide">
    <div class="page__row page__row_head" v-if="!mobile">
      <div
        class="
          page__col
          col__header
          d-flex
          flex-column flex-sm-row
          justify-content-space-between
        "
      >
        <div class="mt-4 mt-sm-0">
          <div class="page__hello h5" v-if="user" v-html="user.name + ','" />
          <div class="page__welcome h2">Привет 👋</div>
        </div>

        <div class="mt-4 mt-sm-0" v-if="items && items.length != 0">
          <p class="mb-2">
            <svg
              width="9"
              height="9"
              viewBox="0 0 14 14"
              fill="none"
              xmlns="http://www.w3.org/2000/svg"
            >
              <circle class="color" cx="7" cy="7" r="6.5" fill="#7FBA7A" />
            </svg>

            <span class="color-gray ml-2 align-middle">На продвижении</span>
          </p>
          <p>
            <svg
              width="9"
              height="9"
              viewBox="0 0 14 14"
              fill="none"
              xmlns="http://www.w3.org/2000/svg"
            >
              <circle class="color" cx="7" cy="7" r="6.5" fill="#FF4242" />
            </svg>

            <span class="color-gray ml-2 align-middle">Есть заявки</span>
          </p>
        </div>
      </div>
    </div>

    <div class="page__row" v-if="mobile && items && items.length != 0">
      <div class="d-flex flex-row align-items-center justify-content-between">
        <h1>Мои треки</h1>
        <img
          :src="url + 'img/icon_plus_main.svg'"
          @click="openMusicModal"
          class="mb-2"
        />
      </div>
    </div>

    <div class="page__row" v-if="mobile && items && items.length != 0">
      <div class="page__panel">
        <p>
          <svg
            width="9"
            height="9"
            viewBox="0 0 12 12"
            fill="none"
            xmlns="http://www.w3.org/2000/svg"
          >
            <circle class="color" cx="6" cy="6" r="5.5" fill="#7FBA7A" />
          </svg>

          <span>На продвижении</span>
        </p>
        <p>
          <svg
            width="9"
            height="9"
            viewBox="0 0 12 12"
            fill="none"
            xmlns="http://www.w3.org/2000/svg"
          >
            <circle
              class="color circle-avatar"
              cx="6"
              cy="6"
              r="5.5"
              fill="#FF4242"
            />
          </svg>

          <span>Есть заявки</span>
        </p>
      </div>
    </div>

    <div class="page__row page__row_border only-desktop">
      <div class="page__col page__col_main-block">
        <div class="products__grid">
          <div
            class="products__item"
            @click="openMusicModal"
            v-if="!mobile && items && items.length > 0"
          >
            <div class="products__preview new"></div>
            <div class="products__details">
              <div class="products__title title">Добавить трек</div>
            </div>
          </div>

          <div
            class="products__item"
            v-for="item in items"
            v-bind:key="item.id"
          >
            <img
              :src="item.image"
              class="products__preview products__image-singl"
            />
            <svg
              class="circle-avatar"
              width="17"
              height="17"
              viewBox="0 0 12 12"
              fill="none"
              xmlns="http://www.w3.org/2000/svg"
            >
              <circle class="color" cx="6" cy="6" r="5.5" fill="#FF4242" />
            </svg>

            <div class="products__details">
              <div class="products__title title">{{ item.author }}</div>
              <div>{{ item.title }}</div>
            </div>
          </div>
        </div>
      </div>
    </div>

    <div v-if="items && items.length === 0" class="page__row page__row_border">
      <div class="page__col">
        <div class="products__banner">
          <article>
            <h5>Загрузи свой первый трек</h5>
            <p>
              Твои будущие фанаты ждут! Жми на&nbsp;кнопку «добавить трек» и
              переходи к&nbsp;продвижению прямо сейчас.
            </p>
            <!-- <div class="products__preview new"></div> -->
            <div class="products__item" @click="openMusicModal">
              <div class="products__icon-wrapper">
                <div class="products__preview new"></div>
              </div>
              <div class="products__details">
                <div class="products__title title">Добавить трек</div>
              </div>
            </div>
          </article>
        </div>
        <div class="products__grid"></div>
      </div>
    </div>

    <b-modal id="music-modal" centered hide-footer>
      <div class="modal-center d-flex flex-column text-center mx-auto">
        <div
          :class="{
            'form-block': true,
            'form-block_loaded': newTrackInfo.photo.length > 0,
          }"
        >
          <img
            class="music-modal-avatar-photo"
            :src="newTrackInfo.photo"
            alt="photo"
            v-if="newTrackInfo.photo.length > 0"
          />

          <!-- ссылка -->
          <p
            v-if="newTrackInfo.photo.length > 0"
            class="music-modal-title-input"
          >
            Ссылка на звук в TikTok
          </p>
          <input
            type="text"
            class="form-control"
            placeholder="Ссылка на звук в TikTok"
            v-model="val"
            required=""
          />
          <!-- название -->
          <p
            v-if="newTrackInfo.title.length > 0"
            class="music-modal-title-input"
          >
            Название
          </p>
          <input
            type="text"
            class="form-control"
            placeholder="Название трека"
            v-model="newTrackInfo.title"
            required=""
            v-if="!!newTrackInfo.title"
          />
          <!-- исполнитель -->
          <p
            v-if="newTrackInfo.author.length > 0"
            class="music-modal-title-input"
          >
            Исполнитель
          </p>
          <input
            type="text"
            class="form-control"
            placeholder="Исполнитель трека"
            v-model="newTrackInfo.author"
            required=""
            v-if="!!newTrackInfo.author"
          />
          <!-- альбом -->
          <p
            v-if="newTrackInfo.album.length > 0"
            class="music-modal-title-input"
          >
            Альбом
          </p>
          <input
            type="text"
            class="form-control form-control_album"
            placeholder="Альбом трека"
            v-model="newTrackInfo.album"
            required=""
            v-if="!!newTrackInfo.album"
          />
          <p class="form-tip text-danger" v-if="error" v-html="error" />
          <button
            class="btn btn-lg btn-primary btn-block my-4"
            @click="getMusic(val)"
            :disabled="!val"
            v-if="!waiting && !newTrackInfo.photo"
            v-html="val ? 'Добавить' : 'Введите ссылку на трек'"
          />
          <!-- url, title, author, album !!-преобразование в булеан -->
          <button
            v-show="!!newTrackInfo.photo && !loading"
            class="btn btn-lg btn-primary btn-block my-4"
            @click="
              sendMusic(
                val,
                newTrackInfo.title,
                newTrackInfo.author,
                newTrackInfo.album
              )
            "
          >
            Добавить
          </button>

          <div class="loading" :class="{ active: waiting }" />
          <p
            class="form-tip"
            v-if="waiting"
            v-html="'Ищем трек, это займет от 5 до 10 секунд'"
          />
        </div>
      </div>
    </b-modal>
  </div>
</template>

<script>
import axios from "axios";
import { mapGetters } from "vuex";
import { GET_MUSIC_LIST, GET_MUSIC, ADD_MUSIC } from "../api-routes";

export default {
  components: {},
  name: "Music",

  data() {
    return {
      editing: false,
      val: this.value,
      music: null,
      error: null,
      waiting: false,
      loading: false,
      items: null,
      newTrackInfo: {
        photo: "",
        title: "",
        author: "",
        album: "",
      },
    };
  },

  mounted() {
    this.getMusicList();
  },

  computed: {
    ...mapGetters(["user", "userAuthorized", "url", "tablet", "mobile"]),
  },

  methods: {
    openMusicModal() {
      this.waiting = this.error = false;
      this.$bvModal.show("music-modal");
    },

    getMusicList() {
      axios.get(GET_MUSIC_LIST).then((response) => {
        this.items = response.data;
      });
    },

    sendMusic(url, title, author, album) {
      console.log(url, title, author, album);
      this.loading = true;
      axios
        .post(ADD_MUSIC, {
          url: url,
          title: title,
          author: author,
          album: album,
        })
        .then((response) => {
          console.log(response);
          this.$bvModal.hide("music-modal");

          this.getMusicList();
          this.clearAreas();
          this.loading = false;

          // this.waiting = false;

          // this.error = null;
        });
    },

    clearAreas() {
      this.newTrackInfo = {
        photo: "",
        title: "",
        author: "",
        album: "",
      };
    },

    getMusic(url) {
      this.waiting = true;
      this.error = null;

      axios
        .post(GET_MUSIC, { url: url })
        .then((response) => {
          this.waiting = false;

          /* TO DO */

          //photo
          console.log(response.data.music.coverThumb);
          this.newTrackInfo.photo = response.data.music.coverThumb;
          //название
          console.log(response.data.music.title);
          this.newTrackInfo.title = response.data.music.title;
          //исполнитель
          console.log(response.data.music.authorName);
          this.newTrackInfo.author = response.data.music.authorName;
          //обложка
          console.log(response.data.music.album);
          this.newTrackInfo.album = response.data.music.album;

          this.error = null;
        })
        .catch((error) => {
          console.log(error);
          this.waiting = false;
          if (error !== undefined) {
            this.error = "Не удалось найти трек, попробуйте еще раз";
          }
        });
    },
  },
};
</script>
