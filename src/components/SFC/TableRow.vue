<template>
  <tr>
    <th class="text-center" scope="row">{{ item.id }}</th>
    <td class="text-center">{{ item.albumId }}</td>
    <td class="text-center">
      <img :src="item.thumbnailUrl" :alt="item.title" class="rounded" />
    </td>
    <td>{{ item.title }}</td>
    <td>
      <a :href="item.url">{{ item.url }}</a>
    </td>
  </tr>
</template>

<script>
export default {
  name: "TableRow",

  props: {
    item: {
      type: Object,
      required: true,
    },
  },
};
</script>

<style scoped>
img {
  width: 48px;
}
</style>
