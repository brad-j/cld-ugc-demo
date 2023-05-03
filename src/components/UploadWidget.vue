<!-- CloudinaryWidget.vue -->

<template>
  <!-- <div class="flex justify-center">
    <button
      @click="openCloudinaryWidget"
      class="bg-[#1865ac] uppercase font-semibold rounded-full px-3 py-2 hover:-translate-y-0.5 active:translate-y-0 transition-all duration-75 my-6"
    >
      Upload Assets
    </button>
  </div>

  <div class="flex justify-center">
    <h3 class="text-xl semi-bold tracking-wide px-3 py-2 rounded-lg">
      Uploaded Assets
      <div class="bg-[#1865ac] w-full h-1 rounded mt-1"></div>
    </h3>
  </div> -->

  <div>
    <div class="flex flex-col items-center min-h-screen bg-neutral-900">
      <button
        @click="openCloudinaryWidget"
        class="bg-cyan-700 text-white py-2 uppercase px-4 rounded mt-8 transform transition duration-300 hover:bg-cyan-600 hover:scale-110 active:scale-95 focus:outline-none"
      >
        Upload Assets
      </button>
      <div v-if="uploadedAssets.length > 0" class="mt-8 w-full">
        <h3 class="text-white text-center">Uploaded Assets:</h3>
        <div class="flex flex-wrap justify-center mt-4">
          <div
            v-for="(asset, index) in uploadedAssets"
            :key="index"
            class="m-2"
          >
            <img
              :src="asset.secure_url"
              :alt="asset.public_id"
              class="w-64 h-64 rounded shadow-lg"
            />
            <p class="text-white mt-2 text-center">{{ asset.public_id }}</p>
          </div>
        </div>
      </div>
    </div>
  </div>

  <!-- <table class="max-w-3xl mx-auto mt-2">
    <thead>
      <tr>
        <th class="text-left py-3 px-4">Public ID</th>
        <th class="text-left py-3 px-4">Format</th>
        <th class="text-left py-3 px-4">URL</th>
      </tr>
    </thead>
    <tbody>
      <tr v-for="(asset, index) in uploadedAssets" :key="index">
        <td class="py-2 px-4">{{ asset.public_id }}</td>
        <td class="py-2 px-4">{{ asset.format }}</td>
        <td class="py-2 px-4">
          <a :href="asset.secure_url" target="_blank" class="text-blue-500">
            URL
          </a>
        </td>
      </tr>
    </tbody>
  </table> -->
</template>

<script>
// import cloudinary from 'cloudinary-upload-widget';

export default {
  data() {
    return {
      uploadedAssets: [],
    };
  },
  methods: {
    openCloudinaryWidget() {
      const widget = cloudinary
        .createUploadWidget(
          {
            cloudName: 'brad-dev',
            apiKey: '216731652951429',
            uploadPreset: 'ugc-demo',
            multiple: true,
            sources: ['local', 'url', 'camera'],
            resourceType: 'image',
            maxFiles: 10,
            maxFilsize: 10485760,
          },
          (error, result) => {
            if (!error && result && result.event === 'success') {
              console.log('Upload Successful:', result.info);
              this.uploadedAssets.push({
                public_id: result.info.public_id,
                folder: result.info.folder,
                format: result.info.format,
                secure_url: result.info.secure_url,
                thumbnail_url: result.info.thumbnail_url,
              });
              // widget.close({ quiet: true });
            } else if (error) {
              console.error('Upload Error:', error);
            }
          },
        )
        .open();
      // .close();
    },
  },
};
</script>

<style>
@import url('https://fonts.googleapis.com/css2?family=Inter:wght@200;400;600&display=swap');
.semi-bold {
  font-weight: 600;
}
</style>
