<template>
  <div>
    <form class="flex flex-col gap-2" @submit.prevent="onSubmit">
      <h2 class="bg-black text-white">ID: {{ item.id }}</h2>
      <input type="text" v-model="corpusName" class="border border-black" />
      <textarea
        v-model="corpusInput"
        class="border border-black"
        placeholder="Enter description here.."
      ></textarea>
      <div class="self-end">
        <p v-if="errorOccured" class="text-red-500">
          Error occured during updating a corpus. Check the internet connection,
          and try again later.
        </p>
        <p v-if="isSaving" class="text-blue-500">Saving...</p>
        <p v-if="saveSucceed" class="text-green-500">Saved successfully.</p>
      </div>
      <div class="flex self-end gap-2">
        <button
          type="button"
          class="bg-red-500 text-white p-2"
          @click="onDelete"
        >
          Delete
        </button>
        <button type="submit" class="bg-blue-500 text-white p-2">Save</button>
      </div>
    </form>
  </div>
</template>
<script>
export default {
  name: "EditBox",
  props: {
    item: Object,
  },
  data() {
    return {
      corpusInput: this.item.attributes.desc,
      corpusName: this.item.attributes.name,
      isSaving: false,
      saveSucceed: false,
      errorOccured: false,
    };
  },
  methods: {
    async onSubmit() {
      this.isSaving = true;
      try {
        await this.$axios.put(
          `${process.env.VUE_APP_STRAPI_URL}/api/corpuses/update-embedding/${this.item.id}`,
          {
            data: {
              name: this.corpusName,
              desc: this.corpusInput,
            },
          }
        );
        this.saveSucceed = true;
        this.errorOccured = false;
      } catch (err) {
        console.log(err);
        this.errorOccured = true;
        this.saveSucceed = false;
      }
      this.isSaving = false;
    },
    async onDelete(){
      try {
        await this.$axios.delete(
          `${process.env.VUE_APP_STRAPI_URL}/api/corpuses/delete-embedding/${this.item.id}`
        );
        this.$router.go()
      } catch (err) {
        console.log(err);
      }
    }
  },
};
</script>
<style></style>
