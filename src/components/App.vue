<template>
  <div class="bg-gray-300 min-h-screen w-screen ">
    <div class=" lg:max-w-6xl mx-auto pt-20 p-5">
      <h1 class="text-3xl text-gray-800 font-bold mb-4">
        Your reminders for a week from now:
      </h1>
      <div class="grid lg:grid-cols-3 gap-5 grid-cols-1 ">
        <div>
          <div
            @click="openReminderModal"
            class="bg-white h-56 rounded-lg p-5 shadow-sm flex justify-center items-center cursor-pointer "
          >
            <p class="text-lg font-medium">
              Create Reminder
            </p>
          </div>
        </div>
        <reminder-card
          v-for="reminder in reminders"
          :key="reminder._id"
          :name="reminder.name"
          :description="reminder.description"
          :datetime="reminder.datetime"
          :color="reminder.color"
        />
      </div>
      <div>
        <!-- <button class="p-2 my-2 bg-gray-100" @click="openReminderModal">
          Create
        </button> -->
      </div>
    </div>

    <reminder-modal
      ref="reminderModal"
      @on-reminder-created="reloadReminders"
    />
  </div>
</template>

<script>
import ReminderModal from "./ReminderModal.vue";
import ReminderCard from "./ReminderCard.vue";

export default {
  components: {
    ReminderCard,
    ReminderModal,
  },
  data: () => ({
    reminders: [],
  }),
  async mounted() {
    await this.reloadReminders();
  },
  methods: {
    async reloadReminders() {
      const now = +new Date();
      const nowPlusWeek = now + 1000 * 60 * 60 * 24 * 7;

      const reminders = await (
        await fetch(`/reminder/get-range?from=${now}&to=${nowPlusWeek}`)
      ).json();
      // Convert date strings to Date
      reminders.forEach((r) => (r.datetime = new Date(r.datetime)));
      this.reminders = reminders;
      console.log(this.reminders, "reminders");
    },
    openReminderModal() {
      this.$refs.reminderModal.open();
    },
  },
};
</script>

<style></style>
