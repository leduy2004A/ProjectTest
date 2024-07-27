<script setup>
import { ref } from "vue";
const dataUser = ref([]);
fetch("https://randomuser.me/api/?page=1&results=10").then(async (data) => {
  dataUser.value = await data.json();
});
const numberPage = ref(1);

function nextData() {
  if (numberPage.value !== 10) {
    numberPage.value += 1;

    fetch(
      `https://randomuser.me/api/?page=${numberPage.value}&results=10`
    ).then(async (data) => {
      dataUser.value = await data.json();
    });
  }
}
function prevData() {
  if (numberPage.value !== 1) {
    numberPage.value -= 1;
    fetch(
      `https://randomuser.me/api/?page=${numberPage.value}&results=10`
    ).then(async (data) => {
      dataUser.value = await data.json();
    });
  }
}
function sortName() {
  dataUser.value.results.sort((a, b) =>
    a.name.first.localeCompare(b.name.first)
  );
  console.log(dataUser.value.results);
}
function sortNamePrev() {
  dataUser.value.results.sort((a, b) =>
    b.name.first.localeCompare(a.name.first)
  );
  console.log(dataUser.value.results);
}
function sortUserName() {
  dataUser.value.results.sort((a, b) =>
    a.login.username.localeCompare(b.login.username)
  );
  console.log(dataUser.value.results);
}
function sortUserNamePrev() {
  dataUser.value.results.sort((a, b) =>
    b.login.username.localeCompare(a.login.username)
  );
  console.log(dataUser.value.results);
}
</script>

<template>
  <table
    class="table-fixed border-spacing-10 border-collapse border border-slate-400"
  >
    <thead>
      <tr>
        <th class="border border-slate-300">
          Fullname
          <span>
            <svg
              class="w-6 h-6 text-gray-800 dark:text-white"
              aria-hidden="true"
              xmlns="http://www.w3.org/2000/svg"
              width="24"
              height="24"
              fill="none"
              viewBox="0 0 24 24"
              @click="sortName()"
            >
              <path
                stroke="currentColor"
                stroke-linecap="round"
                stroke-linejoin="round"
                stroke-width="2"
                d="m5 15 7-7 7 7"
              />
            </svg>
          </span>
          <span>
            <svg
              class="w-6 h-6 text-gray-800 dark:text-white"
              aria-hidden="true"
              xmlns="http://www.w3.org/2000/svg"
              width="24"
              height="24"
              fill="none"
              viewBox="0 0 24 24"
              @click="sortNamePrev()"
            >
              <path
                stroke="currentColor"
                stroke-linecap="round"
                stroke-linejoin="round"
                stroke-width="2"
                d="m19 9-7 7-7-7"
              />
            </svg>
          </span>
        </th>
        <th class="border border-slate-300">
          Username
          <span>
            <svg
              class="w-6 h-6 text-gray-800 dark:text-white"
              aria-hidden="true"
              xmlns="http://www.w3.org/2000/svg"
              width="24"
              height="24"
              fill="none"
              viewBox="0 0 24 24"
              @click="sortUserName()"
            >
              <path
                stroke="currentColor"
                stroke-linecap="round"
                stroke-linejoin="round"
                stroke-width="2"
                d="m5 15 7-7 7 7"
              />
            </svg>
          </span>
          <span>
            <svg
              class="w-6 h-6 text-gray-800 dark:text-white"
              aria-hidden="true"
              xmlns="http://www.w3.org/2000/svg"
              width="24"
              height="24"
              fill="none"
              viewBox="0 0 24 24"
              @click="sortUserNamePrev()"
            >
              <path
                stroke="currentColor"
                stroke-linecap="round"
                stroke-linejoin="round"
                stroke-width="2"
                d="m19 9-7 7-7-7"
              />
            </svg>
          </span>
        </th>
        <th class="border border-slate-300">Thumbnail Icon</th>
      </tr>
    </thead>
    <tbody>
      <tr v-for="item in dataUser.results">
        <td class="border border-slate-300 text-center w-32">
          {{ item.name.title + " " + item.name.first + " " + item.name.last }}
        </td>
        <td class="border border-slate-300 text-center w-32">
          {{ item.login.username }}
        </td>
        <td class="border border-slate-300 text-center w-32">
          <img :src="item.picture.thumbnail" alt="" />
        </td>
      </tr>
    </tbody>
  </table>
  <div class="pagination-ui">
    <div class="flex flex-col items-center">
      <!-- Help text -->
      <span class="text-sm text-gray-700 dark:text-gray-400">
        Showing
        <span class="font-semibold text-gray-900 dark:text-white">{{
          numberPage
        }}</span>
        of
        <span class="font-semibold text-gray-900 dark:text-white">10 pages</span>
      </span>
      <!-- Buttons -->
      <div class="inline-flex mt-2 xs:mt-0">
        <button
          class="flex items-center justify-center px-3 h-8 text-sm font-medium text-white bg-gray-800 rounded-s hover:bg-gray-900 dark:bg-gray-800 dark:border-gray-700 dark:text-gray-400 dark:hover:bg-gray-700 dark:hover:text-white mr-2"
          @click="prevData()"
        >
          Prev
        </button>
        <button
          class="flex items-center justify-center px-3 h-8 text-sm font-medium text-white bg-gray-800 border-0 border-s border-gray-700 rounded-e hover:bg-gray-900 dark:bg-gray-800 dark:border-gray-700 dark:text-gray-400 dark:hover:bg-gray-700 dark:hover:text-white"
          @click="nextData()"
        >
          Next
        </button>
      </div>
    </div>
  </div>
</template>

<style scoped>
.read-the-docs {
  color: #888;
}
table {
  width: 1000px;
}
</style>
