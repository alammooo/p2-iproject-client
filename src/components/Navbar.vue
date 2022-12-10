<script>
import { mapActions, mapWritableState } from "pinia"
import { useCounterStore } from "../stores/counter"

export default {
  data() {
    return {
      active: "text-amber-500",
      buttonClass: {
        "hover:underline": true,
        "duration-200": true,
        "cursor-pointer": true,
        "select-none": true,
        "active:scale-90": true,
      },
      buttonClass1: {
        "hover:underline": true,
        "duration-200": true,
        "cursor-pointer": true,
        "select-none": true,
        "active:scale-90": true,
      },
      buttonClass2: {
        "hover:underline": true,
        "duration-200": true,
        "cursor-pointer": true,
        "select-none": true,
        "active:scale-90": true,
      },
      buttonClass3: {
        "hover:underline": true,
        "duration-200": true,
        "cursor-pointer": true,
        "select-none": true,
        "active:scale-90": true,
      },
    }
  },
  methods: {
    disableAll() {
      this.buttonClass["text-amber-500"] =
        this.buttonClass1["text-amber-500"] =
        this.buttonClass2["text-amber-500"] =
        this.buttonClass3["text-amber-500"] =
          false
    },
    navLogin() {
      this.$router.push("/login")
    },
    navHome() {
      this.$router.push("/")
      this.active
      this.buttonClass["text-amber-500"] = true
    },
    navDetails() {
    this.$router.push("/user-detail")
  },
    navFavorite() {
      this.disableAll()
      this.buttonClass2["text-amber-500"] = true
      this.$router.push("/favoriteList")
    },
    navArticles() {
      this.disableAll()
      this.buttonClass3["text-amber-500"] = true
      this.$router.push("/news")
    },
    navData() {
      this.disableAll()
      this.buttonClass1["text-amber-500"] = true
      this.$router.push("/covid-table")
    },
    ...mapActions(useCounterStore, ["logout", "fetchUserDetail"]),
  },

  created() {
    if (localStorage.getItem("access_token")) {
      this.loggedIn = true
    }
    this.fetchUserDetail()
  },
  updated() {
    this.fetchUserDetail()
  },
  computed: {
    ...mapWritableState(useCounterStore, [
      "loggedIn",
      "userDetail",
      "baseUrl",
      "googleEmail",
      "profilePict",
      "activePage",
    ]),
  },
}
</script>
<template>
  <div class="fixed top-0 w-full z-20 shadow-lg">
    <nav class="bg-white border-gray-200">
      <div
        class="flex flex-wrap justify-between items-center mx-auto max-w-screen-xl px-4 md:px-6 py-2.5">
        <a
          class="flex items-center duration-200 cursor-pointer select-none"
          @click.prevent="navHome">
          <img
            src="../assets/images/logo-medicalinia.jpeg"
            class="h-6 mr-3 sm:h-9"
            alt="Medicalinia Logo" />
          <span class="self-center text-xl font-semibold whitespace-nowrap"
            >Medicalinia</span
          >
        </a>
        <div class="flex items-center gap-5">
          <h1 class="">Welcome, {{ userDetail.fullName }}</h1>
          <div
            class="overflow-hidden relative w-10 h-10 bg-gray-100 rounded-lg dark:bg-gray-600">
            <svg
              v-if="!profilePict"
              class="absolute -left-1 w-12 h-12 text-gray-400"
              fill="currentColor"
              viewBox="0 0 20 20"
              xmlns="http://www.w3.org/2000/svg">
              <path
                fill-rule="evenodd"
                d="M10 9a3 3 0 100-6 3 3 0 000 6zm-7 9a7 7 0 1114 0H3z"
                clip-rule="evenodd"></path>
            </svg>
            <img
              v-else
              :src="`${this.baseUrl}/avatar/${userDetail.profilePict}`"
              alt=""
              class="w-full h-full object-cover" />
          </div>
        </div>
      </div>
    </nav>
    <nav class="bg-gray-50">
      <div class="max-w-screen-xl px-4 py-3 mx-auto md:px-6">
        <div class="flex items-center justify-between">
          <div class="flex gap-5">
            <a @click.prevent="navHome" :class="buttonClass" aria-current="page">Home</a>
            <a @click.prevent="navFavorite" :class="buttonClass2">Favorites</a>
            <a @click.prevent="navArticles" :class="buttonClass3">Articles</a>
            <a @click.prevent="navData" :class="buttonClass1">Chart & Data</a>
          </div>
          <div class="flex gap-5">
            <a
              @click.prevent="navDetails"
              class="font-medium text-blue-600 hover:underline duration-200 cursor-pointer select-none"
              >User Details</a
            >
            <a
              v-if="!loggedIn"
              @click.prevent="navLogin"
              class="font-medium text-blue-600 hover:underline duration-200 cursor-pointer select-none"
              >Login</a
            >
            <a
              v-else
              @click.prevent="logout"
              class="font-medium text-blue-600 hover:underline duration-200 cursor-pointer select-none"
              >Logout</a
            >
          </div>
        </div>
      </div>
    </nav>
  </div>
</template>
