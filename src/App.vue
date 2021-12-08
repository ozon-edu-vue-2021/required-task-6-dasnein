<template>
  <div id="app" class="container my-5">
    <ErrorBlock v-if="error" />
    <template v-else>
      <TableControls :pagination="paginationTable" @set-pagination="setPagination" />

      <Table :items="items" />
      <TablePagination
        v-if="paginationTable"
        :current-page="page"
        :max-page="maxPageNumber"
        @select-page="onPageSelect"
      />
      <IntersectionObserver v-else :active="!loading && !isMaxPage" @intersecting="getNextPage" />
    </template>

    <Spinner v-if="!paginationTable && loading" />
  </div>
</template>

<script>
import Table from "./components/SFC/Table.vue";
import Spinner from "./components/SFC/Spinner.vue";
import ErrorBlock from "./components/SFC/ErrorBlock.vue";
import IntersectionObserver from "./components/SFC/IntersectionObserver.vue";
import TablePagination from "./components/SFC/TablePagination.vue";
import TableControls from "./components/SFC/TableControls.vue";

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
    TablePagination,
    TableControls,
  },

  data() {
    return {
      items: [],
      loading: true,
      error: false,

      limit: 10,
      page: 0,

      paginationTable: true,
    };
  },

  computed: {
    maxPageNumber() {
      return Math.ceil(MAX_TABLE_ITEMS / this.limit);
    },
    isMaxPage() {
      return this.page >= this.maxPageNumber;
    },
  },

  methods: {
    init() {
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
    async fetchData({ limit, page } = this) {
      const queryParams = new URLSearchParams();
      queryParams.append("_limit", limit);
      queryParams.append("_page", page);

      const url = `${API_URL}?${queryParams.toString()}`;

      this.loading = true;

      try {
        const request = await fetch(url);
        const data = await request.json();

        if (this.paginationTable) {
          this.items = data;
        } else {
          this.items = [...this.items, ...data];
        }
      } catch (error) {
        this.error = true;
      }

      this.loading = false;
    },
    getNextPage() {
      this.page += 1;
      this.fetchData();
    },
    getPrevPage() {
      this.page -= this.page > 0 ? 1 : 0;
      this.fetchData();
    },
    onPageSelect(targetPage) {
      if (targetPage > 0 && targetPage <= this.maxPageNumber && this.page !== targetPage) {
        this.page = targetPage;
        window.scrollTo({ top: 0 });
        this.fetchData();
      }
    },
    setPagination(val) {
      this.paginationTable = val;
      this.page = 0;
      this.items = [];
      this.init();
    },
  },

  created() {
    this.init();
  },
};
</script>
