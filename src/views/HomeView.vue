<template>
  <div class="row">
    <div v-for="image of images" class="col-12 col-md-3 mb-3">
      <img :src="image" class="img-fluid image-preview" alt="" />
    </div>
  </div>
</template>

<script setup>
import { onMounted, onUnmounted, reactive } from "vue";

const images = reactive([]);

const handlePaste = async () => {
  const items = await navigator.clipboard.read().catch((err) => {
    console.log(err);
  });

  for (let item of items) {
    for (let type of item.types) {
      if (type.startsWith("image/")) {
        item.getType(type).then((imageBlob) => {
          const url = window.URL.createObjectURL(imageBlob);
          images.push(url);
        });
      }
    }
  }
};

onMounted(() => {
  document.addEventListener("paste", handlePaste);
});

onUnmounted(() => {
  document.removeEventListener("paste", handlePaste);
});
</script>

<style>
.image-preview {
  border: 1px solid black;
  box-shadow: 3px 3px 0 black;
}
</style>
