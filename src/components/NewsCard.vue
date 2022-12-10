<script>
import { mapActions, mapWritableState } from "pinia"
import { useCounterStore } from "../stores/counter"
import PictureLoader from "./PictureLoader.vue"
import Spinner from "./Spinner.vue";
export default {
  components: {
    PictureLoader,
    Spinner
},
  props: ["news"],
  methods: {
    ...mapActions(useCounterStore, ["addFavorites"]),
  },
  computed: {
    // dateFormatter() {
    //   return this.news.publishedAt.toLocaleDateString("en-CA")
    // },
    ...mapWritableState(useCounterStore, ["loadPict", "isSpinner"]),
  },
}
</script>
<template>
  <PictureLoader v-if="loadPict" />
  <div
    v-else
    class="relative overflow-hidden bg-no-repeat bg-cover shadow-lg rounded-lg"
    style="background-position: 50%"
    data-mdb-ripple="true"
    data-mdb-ripple-color="light">
    <img :src="news.urlToImage" class="w-full" />
    <a class="cursor-pointer">
      <div
        class="absolute top-0 right-0 bottom-0 left-0 w-full h-full overflow-hidden bg-fixed"
        style="background-color: rgba(0, 0, 0, 0.4)">
        <div class="flex justify-start items-end h-full">
          <div class="text-white m-6">
            <a :href="news.url" target="_blank" class="font-bold text-lg mb-3">{{
              news.title
            }}</a>
            <p>
              <small
                >Published <u>{{ news.publishedAt }}</u> by {{ news.author }}</small
              >
            </p>
          </div>
          <button
            @click.prevent="
              addFavorites({
                title: news.title,
                description: news.description,
                urlToImage: news.urlToImage,
                url: news.url,
              })
            "
            class="py-2 px-5 text-sm font-medium text-slate-900 bg-slate-50 rounded-br-md rounded-tl-md hover:bg-amber-100 hover:text-blue-700">
            ❤️
          </button>
        </div>
      </div>
    </a>
  </div>
</template>
