<template>
  <div>Home</div>
  <ul class="flex flex-col gap-6 p-2">
    <li v-for="item in items" :key="item.id">
      <EditBox :item="item" />
    </li>
  </ul>
  <CreateBox class="mt-6" />
</template>
<script>
import EditBox from "../EditBox.vue";
import CreateBox from "../CreateBox.vue";

export default {
  name: "HomeView",
  components: { EditBox, CreateBox },
  data() {
    return {
      items: [],
    };
  },
  methods: {
    async fetchCorpuses() {
      const response = await this.$axios.get(
        `${process.env.VUE_APP_STRAPI_URL}/api/corpuses`
      );
      return response.data.data;
    },
  },
  async mounted() {
    try {
      let items = await this.fetchCorpuses();
      items.sort((a, b) => a.id - b.id);
      this.items = items;
    } catch (err) {
      console.log(err);
    }
  },
};
</script>
<style></style>
