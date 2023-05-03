<script>
import UploadWidget from './components/UploadWidget.vue';
export default {
  name: 'App',
  components: {
    UploadWidget: UploadWidget,
  },
};
</script>

<template>
  <div
    class="bg-gradient-to-b from-neutral-950 to-neutral-800 text-white font h-screen"
  >
    <h1
      class="text-4xl tracking-wide uppercase font-siemibold pt-6 pb-3 text-center"
    >
      Cloudinary UGC Demo
    </h1>

    <div>
      <UploadWidget />
    </div>

    <div>
      <img id="uploadedimage" src="" />
    </div>
  </div>
</template>

<style>
@import url('https://fonts.googleapis.com/css2?family=Inter:wght@200;400;600&display=swap');
.font {
  font-family: 'Inter', sans-serif;
}
</style>
