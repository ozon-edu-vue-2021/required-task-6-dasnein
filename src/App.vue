<template>
  <div id="app" class="container my-5">
    <ErrorBlock v-if="error" />
    <template v-else>
      <Table :items="items" />
      <IntersectionObserver
        v-if="!paginationTable"
        @intersecting="getNextPage"
        :active="!loading && !isMaxItemsLoaded"
      />
    </template>
    <Spinner v-if="loading" />
  </div>
</template>

<script>
import Table from "./components/SFC/Table.vue";
// import Test from "./components/Test.vue";
import Spinner from "./components/SFC/Spinner.vue";
import ErrorBlock from "./components/SFC/ErrorBlock.vue";
import IntersectionObserver from "./components/SFC/IntersectionObserver.vue";

const API_URL = "https://jsonplaceholder.typicode.com/photos";
const TABLE_ROW_HEIGHT = 65;
const MAX_TABLE_ITEMS = 5000; // this is jsonplaceholder.typicode.com limit for photos API

export default {
  name: "App",

  components: {
    Table,
    Spinner,
    ErrorBlock,
    IntersectionObserver,
  },

  data() {
    return {
      items: [],
      loading: true,
      error: false,

      limit: 10,
      page: 0,

      paginationTable: false,
    };
  },

  computed: {
    isMaxItemsLoaded() {
      return this.page >= Math.ceil(MAX_TABLE_ITEMS / this.limit);
    },
  },

  methods: {
    async fetchData({ limit, page } = this) {
      const queryParams = new URLSearchParams();
      queryParams.append("_limit", limit);
      queryParams.append("_page", page);

      const url = `${API_URL}?${queryParams.toString()}`;

      this.loading = true;

      try {
        const request = await fetch(url);
        const data = await request.json();

        this.items = [...this.items, ...data];
      } catch (error) {
        this.error = true;
      }

      this.loading = false;
    },
    getNextPage() {
      this.page += 1;
      this.fetchData();
    },
  },

  created() {
    const windowHeight = window.innerHeight;

    let firstRequestPagesQuantity = 0;
    let prefetchedTableHeight = 0;

    while (prefetchedTableHeight < windowHeight) {
      firstRequestPagesQuantity += 1;
      prefetchedTableHeight = firstRequestPagesQuantity * this.limit * TABLE_ROW_HEIGHT;
    }

    this.page = firstRequestPagesQuantity;

    this.fetchData({
      limit: firstRequestPagesQuantity * this.limit,
      page: 0,
    });
  },
};
</script>
