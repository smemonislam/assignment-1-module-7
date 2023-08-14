<script setup>
import { ref, onMounted, onBeforeUnmount } from "vue";

const newLabel = ref("");
const newItem = ref(16);
let chart = null;
const dataset = [300, 50, 100];

const data = {
  labels: ["Red", "Blue", "Yellow"],
  datasets: [
    {
      label: "My First Dataset",
      data: dataset,
      backgroundColor: ["Red", "Blue", "Yellow"],
      hoverOffset: 4,
    },
  ],
};

const config = {
  type: "pie",
  data: data,
};

onMounted(() => {
  const ctx = document.getElementById("chart");
  chart = new Chart(ctx, config);
});

const addItems = () => {
  if ("" !== newItem.value && "" !== newLabel.value) {
    dataset.push(newItem.value);
    data.labels.push(newLabel.value);

    chart.data.datasets[0].data = dataset;
    data.datasets[0].backgroundColor.push(newLabel.value.toLowerCase());
    chart.update();
  }
};

onBeforeUnmount(() => {
  chart.destroy();
});
</script>
<template>
  <div class="container mt-4">
    <div class="row">
      <div class="col-md-8 offset-md-2">
        <div class="mx-auto w-[400px] h-[400px] bg-gray-400">
          <canvas id="chart"> </canvas>
        </div>
      </div>
    </div>
  </div>
  <div class="container mt-3">
    <div class="row">
      <div class="col-md-8 offset-md-2">
        <form class="row g-3" @submit.prevent>
          <div class="col-auto">
            <label for="inputColor" class="visually-hidden">Color Label</label>
            <input
              type="text"
              class="form-control-plaintext"
              id="inputColor"
              placeholder="Color Label"
              v-model="newLabel"
            />
          </div>
          <div class="col-auto">
            <label for="inputDataset" class="visually-hidden">Dataset</label>
            <input
              type="text"
              class="form-control"
              id="inputDataset"
              placeholder="Dataset"
              v-model="newItem"
            />
          </div>
          <div class="col-auto">
            <button
              type="submit"
              class="btn btn-info mb-3"
              @click.prevent="addItems()"
            >
              <i class="fa-solid fa-plus"></i> Add
            </button>
          </div>
        </form>
      </div>
    </div>
  </div>
</template>
<style></style>
