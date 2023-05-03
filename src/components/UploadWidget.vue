<template>
  <div>
    <div class="flex flex-col items-center min-h-screen bg-neutral-900">
      <button
        @click="openCloudinaryWidget"
        class="bg-cyan-700 text-white py-2 uppercase px-4 rounded mt-8 transform hover:bg-cyan-600 hover:scale-105 active:scale-100 focus:outline-none transition-all duration-75"
      >
        Upload Assets
      </button>

      <div class="flex flex-col items-center min-h-screen">
        <div v-if="uploadedAssets.length > 0" class="mt-8 w-full">
          <h3 class="text-xl text-center tracking-wider uppercase">
            Uploaded Assets
          </h3>
          <div
            class="grid grid-cols-1 sm:grid-cols-2 md:grid-cols-3 gap-4 mt-4 px-4"
          >
            <div
              v-for="(asset, index) in uploadedAssets"
              :key="index"
              class="m-2"
            >
              <img
                :src="asset.thumbnail_url"
                :alt="asset.public_id"
                class="w-full h-auto rounded shadow-lg"
              />
              <p class="text-white mt-2 text-center">{{ asset.public_id }}</p>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      uploadedAssets: [],
      dragging: false, // Add this line
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
                thumbnail_url: this.generateThumbnailUrl(
                  result.info.secure_url,
                ),
              });
              // widget.close({ quiet: true });
            } else if (error) {
              console.error('Upload Error:', error);
            }
          },
        )
        .open();
      if (files) {
        setTimeout(() => {
          for (let i = 0; i < files.length; i++) {
            sendFile(files[i]);
          }
        }, 1000); // Adjust the delay as needed
      }
    },

    generateThumbnailUrl(secureUrl) {
      const transformation = 'w_300,h_300,c_fill,ar_1:1';
      const urlParts = secureUrl.split('/');
      urlParts.splice(-2, 0, transformation);

      return urlParts.join('/');
    },
  },
};
</script>

<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Inter:wght@200;400;600&display=swap');
.semi-bold {
  font-weight: 600;
}
/* Override default Cloudinary widget styles */
.cloudinary-widget .drag-area {
  background-color: #2d3748 !important;
  border: 2px dashed #319795 !important;
}

.cloudinary-widget .drag-area:hover {
  background-color: #2c5282 !important;
  border-color: #48bb78 !important;
}
</style>
