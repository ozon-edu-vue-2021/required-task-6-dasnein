<template>
  <nav aria-label="Page navigation example">
    <ul class="pagination justify-content-center">
      <li class="page-item" :class="{ disabled: isMinPage }">
        <a class="page-link" href="#" @click.prevent="prevPage">
          <span aria-hidden="true">&laquo;</span>
        </a>
      </li>

      <li
        v-for="page, index in pagesList"
        class="page-item"
        :class="{ active: currentPage === +page, disabled: page === '...' }"
        :key="`table-page-${page}-${index}`"
      >
        <a v-if="page === '...'" class="page-link">{{ page }}</a>
        <a v-else class="page-link" href="#" @click.prevent="selectPage(+page)">{{ page }}</a>
      </li>

      <li class="page-item" :class="{ disabled: isMaxPage }">
        <a class="page-link" href="#" @click.prevent="nextPage">
          <span aria-hidden="true">&raquo;</span>
        </a>
      </li>
    </ul>
  </nav>
</template>

<script>
export default {
  name: "TablePagination",

  props: {
    currentPage: {
      type: Number,
      required: true,
    },
    maxPage: {
      type: Number,
      required: true,
    },
  },

  computed: {
    isMaxPage() {
      return this.currentPage === this.maxPage;
    },
    isMinPage() {
      return this.currentPage === 1;
    },
    pagesList() {
      const startPage = this.currentPage > 3 ? this.currentPage - 3 : 1;
      const endPage = this.currentPage < this.maxPage - 3 ? this.currentPage + 3 : this.maxPage;

      const pages = [];

      if (startPage > 1) {
        pages.push(1);

        if (startPage > 2) {
          pages.push("...");
        }
      }

      for (let i = startPage; i <= endPage; i += 1) {
        pages.push(i);
      }

      if (endPage < this.maxPage - 1) {
        pages.push("...");
      }

      if (endPage < this.maxPage) {
        pages.push(this.maxPage);
      }

      return pages;
    },
  },

  methods: {
    nextPage() {
      if (this.currentPage < this.maxPage) {
        this.selectPage(this.currentPage + 1);
      }
    },
    prevPage() {
      if (this.currentPage > 1) {
        this.selectPage(this.currentPage - 1);
      }
    },
    selectPage(n) {
      if (n !== this.currentPage) {
        this.$emit("select-page", n);
      }
    },
  },
};
</script>
