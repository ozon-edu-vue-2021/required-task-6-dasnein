<template>
  <table class="table table-bordered">
    <TableHead />
    <tbody>
      <TableRow v-for="item in items" :key="`row-${item.id}`" :item="item" />
    </tbody>
  </table>
</template>

<script>
import TableHead from "./TableHead.vue";
import TableRow from "./TableRow.vue";

export default {
  name: "Table",

  components: {
    TableHead,
    TableRow,
  },

  props: {
    items: {
      type: Array,
      required: true,
    },
  },
};
</script>
