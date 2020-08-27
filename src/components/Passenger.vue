<template>
  <div class=" bg-white text-gray-600 px-12 py-8 mb-8 rounded-lg shadow-lg">
    <div class="mb-2"><strong>Name: </strong>{{ data.name }}</div>
    <div><strong>Trips:</strong> {{ data.trips }}</div>
  </div>
</template>

<script>
export default {
  name: "Passenger",
  props: {
    data: {
      type: Object,
      required: true,
      default: () => {
        return {};
      },
    },
  },
};
</script>
