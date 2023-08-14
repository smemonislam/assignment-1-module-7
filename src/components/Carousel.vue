<script setup>
import { ref, reactive, onMounted, onBeforeUnmount } from "vue";
const isActive = ref(0);

let newItems = reactive({ thumb: "", title: "", content: "" });

const images = reactive([
  {
    thumb:
      "https://images.unsplash.com/photo-1682685797769-481b48222adf?ixlib=rb-4.0.3&ixid=M3wxMjA3fDF8MHxlZGl0b3JpYWwtZmVlZHwxfHx8ZW58MHx8fHx8&auto=format&fit=crop&w=300&q=60",
    title: "First slide label",
    content: "Some representative placeholder content for the first slide.",
  },
  {
    thumb:
      "https://images.unsplash.com/photo-1682695794816-7b9da18ed470?ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxwaG90by1yZWxhdGVkfDZ8fHxlbnwwfHx8fHw%3D&auto=format&fit=crop&w=300&q=60",
    title: "Second slide label",
    content: "Some representative placeholder content for the first slide.",
  },
  {
    thumb:
      "https://images.unsplash.com/photo-1682685797661-9e0c87f59c60?ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxwaG90by1yZWxhdGVkfDE4fHx8ZW58MHx8fHx8&auto=format&fit=crop&w=300&q=60",
    title: "Third slide label",
    content: "Some representative placeholder content for the first slide.",
  },
  {
    thumb:
      "https://images.unsplash.com/photo-1682685797828-d3b2561deef4?ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D&auto=format&fit=crop&w=1170&q=80",
    title: "Fourth slide label",
    content: "Some representative placeholder content for the first slide.",
  },
]);

const prevSlide = () => {
  isActive.value <= 0 ? (isActive.value = images.length - 1) : isActive.value--;
};

const nextSlide = () => {
  isActive.value >= images.length - 1 ? (isActive.value = 0) : isActive.value++;
};

const indicators = (index) => {
  isActive.value = index;
};

const addSliderItem = () => {
  if ("" != newItems.thumb) {
    images.push(newItems);
  }
};

onMounted(() => {
  autoSlideTimer = setInterval(nextSlide, 3000);
});

onBeforeUnmount(() => {
  // Clean up ongoing tasks or resources here
  if (autoSlideTimer) {
    clearInterval(autoSlideTimer);
  }
});

let autoSlideTimer = null;
</script>

<template>
  <div class="container">
    <div class="row">
      <div class="col-md-8 offset-md-2">
        <form @submit.prevent>
          <div class="form-group row mb-3">
            <label for="inputUrl" class="col-sm-2 col-form-label"
              >Image Url</label
            >
            <div class="col-sm-10">
              <input
                type="url"
                class="form-control"
                id="inputUrl"
                v-model="newItems.thumb"
                required
              />
            </div>
          </div>
          <div class="form-group row mb-3">
            <label for="inputTitle" class="col-sm-2 col-form-label"
              >Title</label
            >
            <div class="col-sm-10">
              <input
                type="text"
                class="form-control"
                id="inputTitle"
                v-model="newItems.title"
              />
            </div>
          </div>
          <div class="form-group row mb-3">
            <label for="inputContent" class="col-sm-2 col-form-label"
              >Content</label
            >
            <div class="col-sm-10">
              <input
                type="text"
                class="form-control"
                id="inputContent"
                v-model="newItems.content"
              />
            </div>
          </div>
          <div class="form-group row mb-3">
            <div class="col-md-10">
              <button
                @click.prevent="addSliderItem()"
                type="submit"
                class="btn btn-info"
              >
                <i class="fa-solid fa-plus"></i> Add New Images
              </button>
            </div>
          </div>
        </form>
      </div>
    </div>
  </div>
  <div class="container">
    <div class="row">
      <div class="col-md-8 offset-md-2">
        <div
          id="carouselExampleCaptions"
          class="carousel slide"
          data-bs-ride="false"
        >
          <div class="carousel-indicators">
            <button
              @click="indicators(index)"
              type="button"
              data-bs-target="#indicators"
              v-for="(image, index) in images"
              :key="index"
              :class="index == isActive ? 'active' : ''"
            ></button>
          </div>
          <div class="carousel-inner">
            <div
              class="carousel-item"
              v-for="(image, index) in images"
              :key="index"
              :class="index == isActive ? 'active' : ''"
            >
              <img
                :src="image.thumb"
                class="d-block w-100 h-571"
                :alt="image.title"
              />
              <div class="carousel-caption d-none d-md-block">
                <h5>{{ image.title }}</h5>
                <p>{{ image.content }}</p>
              </div>
            </div>
          </div>
          <button
            @click="prevSlide"
            class="carousel-control-prev"
            type="button"
            data-bs-target="#carouselExampleCaptions"
            data-bs-slide="prevSlide"
          >
            <span class="carousel-control-prev-icon" aria-hidden="true"></span>
            <span class="visually-hidden">Previous</span>
          </button>
          <button
            @click="nextSlide"
            class="carousel-control-next"
            type="button"
            data-bs-target="#carouselExampleCaptions"
            data-bs-slide="nextSlide"
          >
            <span class="carousel-control-next-icon" aria-hidden="true"></span>
            <span class="visually-hidden">Next</span>
          </button>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>
.h-571 {
  height: 571px;
}
</style>
