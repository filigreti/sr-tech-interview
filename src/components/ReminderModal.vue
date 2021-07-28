<template>
  <dialog ref="dialog" class="border border-black rounded-md shadow-md">
    <div class="flex flex-col">
      <button class="absolute top-0 right-0 m-2 p-2" @click="close">🗙</button>
      <h2 class="text-2xl text-gray-800 font-bold mb-4">New reminder</h2>
      <input class="p-2 my-2" type="text" v-model="name" placeholder="name" />
      <textarea
        class="p-2 my-2"
        type="text"
        v-model="description"
        placeholder="description"
      />
      <input class="p-2 my-2" type="datetime-local" v-model="datetime" />
      <div class="flex  flex-col ">
        <p class="text-sm py-2">Select Reminder Color</p>
        <div class="flex items-center">
          <div
            @click="selectedColor = color"
            v-for="color in colors"
            :key="color"
            :class="color"
            class=" h-6 w-6  rounded-md shadow mr-4 cursor-pointer"
          ></div>
        </div>
      </div>
      <voice-recorder />
      <button class="p-2 my-2 bg-gray-100" @click="create">Create</button>
    </div>
  </dialog>
</template>

<script>
import VoiceRecorder from "./VoiceRecorder.vue";

export default {
  components: { VoiceRecorder },
  data: () => ({
    isOpen: true,
    name: "",
    description: "",
    datetime: new Date().toLocaleString("sv").replace(" ", "T"),
    colors: ["bg-green-500", "bg-yellow-500", "bg-red-500"],
    selectedColor: null,
  }),
  mounted() {},
  methods: {
    open() {
      this.name = "";
      this.description = "";
      this.datetime = new Date().toLocaleString("sv").replace(" ", "T");
      this.$refs.dialog.showModal();
    },
    close() {
      this.$refs.dialog.close();
    },
    async create() {
      const res = await (
        await fetch("/reminder/create", {
          method: "POST",
          headers: { "Content-Type": "application/json" },
          body: JSON.stringify({
            name: this.name,
            description: this.description,
            datetime: this.datetime,
            color: this.selectedColor,
          }),
        })
      ).json();
      if (res.err) {
        window.alert(res.err);
        return;
      }
      this.$emit("on-reminder-created");
      this.close();
    },
  },
};
</script>

<style scoped></style>
