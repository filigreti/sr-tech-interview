<template>
  <div
    :class="color ? color : 'bg-white'"
    class=" h-56 rounded-lg p-5 shadow-sm  "
  >
    <h3 class="text-base font-medium">{{ name }}</h3>
    <p>{{ datetime }}</p>
    <p>{{ description }}</p>
  </div>
</template>

<script>
export default {
  props: {
    name: {
      type: String,
      required: true,
    },
    description: {
      type: String,
      required: true,
    },
    datetime: {
      type: Date,
      required: true,
    },
    color: {},
  },
  mounted() {},
  methods: {},
};
</script>

<style scoped></style>
