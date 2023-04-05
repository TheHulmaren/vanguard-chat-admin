<template>
  <div>
    <form class="flex flex-col gap-2 p-2 bg-blue-300" @submit.prevent="onSubmit">
      <h2 class="bg-black text-white">New</h2>
      <input type="text" v-model="corpusName" class="border border-black" />
      <textarea
        v-model="corpusInput"
        class="border border-black"
        placeholder="Enter description here.."
      ></textarea>
      <div class="self-end">
        <p v-if="errorOccured" class="text-red-500">
          Error occured during creating a new corpus. Check the internet connection,
          and try again later.
        </p>
        <p v-if="isSaving" class="text-blue-500">Creating...</p>
        <p v-if="saveSucceed" class="text-green-500">Created successfully.</p>
      </div>
      <button type="submit" class="bg-blue-500 text-white self-end p-2">
        Save
      </button>
    </form>
  </div>
</template>
<script>
export default {
  name: "EditBox",
  data() {
    return {
      corpusInput: '',
      corpusName: '',
      isSaving: false,
      saveSucceed: false,
      errorOccured: false,
    };
  },
  methods: {
    async onSubmit() {
      this.isSaving = true;
      console.log(process.env.VUE_APP_STRAPI_URL)
      try {
        await this.$axios.post(
          `${process.env.VUE_APP_STRAPI_URL}/api/corpuses/add-embedding`,
          {
            data: {
              name: this.corpusName,
              desc: this.corpusInput,
            },
          }
        );
        this.saveSucceed = true;
        this.errorOccured = false;

        this.$router.go()
      } catch (err) {
        console.log(err);
        this.errorOccured = true;
        this.saveSucceed = false;
      }
      this.isSaving = false;
    },
  },
};
</script>
<style></style>
