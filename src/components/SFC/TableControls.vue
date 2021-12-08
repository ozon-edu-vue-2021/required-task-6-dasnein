<template>
  <div class="mb-4">
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
</template>

<script>
export default {
  name: "TableControls",

  props: {
    pagination: {
      type: Boolean,
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
  },
};
</script>
