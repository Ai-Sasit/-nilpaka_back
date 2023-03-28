<template>
  <header>
    <nav
      class="bg-white border-gray-200 px-4 lg:px-6 py-2.5 dark:bg-gray-800"
      style="box-shadow: rgba(99, 99, 99, 0.2) 0px 2px 8px 0px">
      <div class="flex flex-wrap justify-between items-center">
        <NuxtLink to="/" class="flex items-center" style="color: back">
          <img
            src="../assets/imgs/ll-01.png"
            class="mr-3 h-6 sm:h-9"
            alt="Flowbite Logo" />
          <span
            class="self-center text-xl font-semibold whitespace-nowrap dark:text-white"
            style="color: black"
            >ระบบจัดการข้อมูลทัวร์</span
          >
        </NuxtLink>
        <div class="flex items-center lg:order-2">
          <v-btn variant="flat" icon="mdi-logout" href="/"></v-btn>
          <button
            @click="drawer = true"
            type="button"
            class="inline-flex items-center p-2 ml-1 text-sm text-gray-500 rounded-lg lg:hidden hover:bg-gray-100 focus:outline-none focus:ring-2 focus:ring-gray-200 dark:text-gray-400 dark:hover:bg-gray-700 dark:focus:ring-gray-600"
            aria-controls="mobile-menu-2"
            aria-expanded="false">
            <span class="sr-only">Open main menu</span>
            <svg
              class="w-6 h-6"
              fill="currentColor"
              viewBox="0 0 20 20"
              xmlns="http://www.w3.org/2000/svg">
              <path
                fill-rule="evenodd"
                d="M3 5a1 1 0 011-1h12a1 1 0 110 2H4a1 1 0 01-1-1zM3 10a1 1 0 011-1h12a1 1 0 110 2H4a1 1 0 01-1-1zM3 15a1 1 0 011-1h12a1 1 0 110 2H4a1 1 0 01-1-1z"
                clip-rule="evenodd"></path>
            </svg>
            <svg
              class="hidden w-6 h-6"
              fill="currentColor"
              viewBox="0 0 20 20"
              xmlns="http://www.w3.org/2000/svg">
              <path
                fill-rule="evenodd"
                d="M4.293 4.293a1 1 0 011.414 0L10 8.586l4.293-4.293a1 1 0 111.414 1.414L11.414 10l4.293 4.293a1 1 0 01-1.414 1.414L10 11.414l-4.293 4.293a1 1 0 01-1.414-1.414L8.586 10 4.293 5.707a1 1 0 010-1.414z"
                clip-rule="evenodd"></path>
            </svg>
          </button>
        </div>

        <div
          class="hidden justify-between items-center w-full lg:flex lg:w-auto lg:order-1"
          id="mobile-menu-2">
          <ul
            style="margin-bottom: 0; align-items: center"
            class="flex flex-col font-medium lg:flex-row lg:space-x-8 lg:mt-0">
            <li>
              <v-btn variant="flat" @click="$router.push('/tour_ls')"
                >รายการทัวร์</v-btn
              >
            </li>
            <li>
              <v-btn variant="flat" @click="$router.push('/addtour')"
                >สร้างทัวร์</v-btn
              >
            </li>
            <li>
              <v-btn variant="flat" @click="$router.push('/dashboard')"
                >รายงาน</v-btn
              >
            </li>
            <li>
              <v-btn variant="flat" @click="$router.push('/createreceipt')"
                >สร้างใบเสร็จรับเงิน</v-btn
              >
            </li>
          </ul>
        </div>
      </div>
    </nav>
  </header>
  <v-layout>
    <v-navigation-drawer v-model="drawer" temporary location="right">
      <v-list-item
        prepend-avatar="https://cdn-icons-png.flaticon.com/512/3251/3251650.png"
        title="Admin นะจ๊ะ"></v-list-item>

      <v-divider></v-divider>

      <v-list density="compact" nav>
        <v-list-item
          prepend-icon="mdi-list-box-outline"
          title="รายการทัวร์"
          value="1"
          href="/tour_ls"></v-list-item>
        <v-list-item
          prepend-icon="mdi-list-box-outline"
          title="สร้างทัวร์"
          value="2"
          href="/addtour"></v-list-item>
        <v-list-item
          prepend-icon="mdi-list-box-outline"
          title="สร้างใบเสร็จรับเงิน"
          value="3"
          href="/createReceipt"></v-list-item>
        <v-list-item
          prepend-icon="mdi-list-box-outline"
          title="รายงาน"
          href="/dashboard"
          value="4"></v-list-item>
      </v-list> </v-navigation-drawer
  ></v-layout>
  <div>
    <main><slot></slot></main>
  </div>
</template>

<script lang="ts">
import { defineComponent } from "vue";
import { initModals, initDropdowns } from "flowbite";
import { signOut } from "@firebase/auth";
export default defineComponent({
  mounted() {
    initModals();
    initDropdowns();
  },
  data() {
    return {
      raw: this.$route.name,
      role: 0,
      role_name: "",
      account: false,
      drawer: false,
      profile_auth: {} as any,
    };
  },
  beforeUpdate() {
    this.raw = this.$route.name;
  },
  methods: {
    logout() {
      signOut(this.$auth).then(() => {
        localStorage.removeItem("token");
        console.log("logout");
        this.$router.push("/login");
      });
    },
  },
});
</script>
