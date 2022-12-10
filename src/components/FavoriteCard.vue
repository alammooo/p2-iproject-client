<script>
import { mapActions, mapWritableState } from "pinia"
import { useCounterStore } from "../stores/counter"
import PictureLoader from "./PictureLoader.vue"

export default {
  components: {
    PictureLoader,
  },
  props: ["favorite"],
  methods: {
    ...mapActions(useCounterStore, ["deleteFavorite"]),
  },
  computed: {
    // dateFormatter() {
    //   return this.news.publishedAt.toLocaleDateString("en-CA")
    // },
    ...mapWritableState(useCounterStore, ["loadPict"]),
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
    <img :src="favorite.urlToImage" class="w-full" />
    <a class="cursor-pointer">
      <div
        class="absolute top-0 right-0 bottom-0 left-0 w-full h-full overflow-hidden bg-fixed"
        style="background-color: rgba(0, 0, 0, 0.4)">
        <div class="flex justify-start items-end h-full">
          <div class="text-white m-6">
            <a :href="favorite.url" target="_blank" class="font-bold text-lg mb-3">{{
              favorite.title
            }}</a>
          </div>
          <button
            @click.prevent="deleteFavorite(favorite.id)"
            class="py-2 px-5 text-sm font-medium text-sky-50 bg-red-500 rounded-br-md rounded-tl-md hover:bg-red-600">
            Delete
          </button>
        </div>
      </div>
    </a>
  </div>
</template>
