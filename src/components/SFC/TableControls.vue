<template>
  <div class="mb-4">
    <div class="row">
      <div class="col">
        <div class="form-check">
          <input
            v-model="enablePagination"
            class="form-check-input"
            type="radio"
            name="flexRadioDefault"
            id="flexRadioDefault1"
            :value="false"
          />
          <label class="form-check-label" for="flexRadioDefault1">Disable pagination</label>
        </div>
        <div class="form-check">
          <input
            v-model="enablePagination"
            class="form-check-input"
            type="radio"
            name="flexRadioDefault"
            id="flexRadioDefault2"
            :value="true"
          />
          <label class="form-check-label" for="flexRadioDefault2">Enable pagination</label>
        </div>
      </div>
    </div>
    <div class="row mt-4 border-top pt-4">
      <div class="col-6">
        <select v-model="sortingField" class="form-select">
          <option :value="null" disabled>Select sorting field</option>
          <option value="id">Sort by ID</option>
          <option value="albumId">Sort by Album ID</option>
          <option value="title">Sort by Title</option>
        </select>
      </div>
      <div class="col-6">
        <div class="form-check">
          <input
            v-model="sortingOrder"
            class="form-check-input"
            type="radio"
            name="flexRadioDefault2"
            id="flexRadioDefault11"
            value="asc"
          />
          <label class="form-check-label" for="flexRadioDefault11">Ascending</label>
        </div>
        <div class="form-check">
          <input
            v-model="sortingOrder"
            class="form-check-input"
            type="radio"
            name="flexRadioDefault2"
            id="flexRadioDefault22"
            value="desc"
          />
          <label class="form-check-label" for="flexRadioDefault22">Descending</label>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "TableControls",

  props: {
    pagination: {
      type: Boolean,
    },
    order: {
      type: String,
    },
    sort: {
      type: String,
    },
  },

  computed: {
    enablePagination: {
      get() {
        return this.pagination;
      },
      set(v) {
        this.$emit("set-pagination", v);
      },
    },
    sortingOrder: {
      get() {
        return this.order;
      },
      set(v) {
        this.$emit("set-order", v);
      },
    },
    sortingField: {
      get() {
        return this.sort;
      },
      set(v) {
        this.$emit("set-sort", v);
      },
    },
  },
};
</script>
