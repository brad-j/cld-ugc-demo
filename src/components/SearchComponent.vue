<template>
  <div class="search-component flex flex-col items-center">
    <input
      v-model="searchQuery"
      @input="searchImages"
      placeholder="Search images..."
      class="w-full p-2 text-black bg-white rounded-md shadow-md focus:outline-none focus:ring-2 focus:ring-indigo-500"
    />
    <div class="search-results flex flex-wrap justify-center mt-5">
      <div
        v-for="(image, index) in searchResults"
        :key="index"
        class="search-result m-2 w-36"
      >
        <img
          :src="image.secure_url"
          :alt="image.public_id"
          class="w-full h-auto rounded-md"
        />
      </div>
    </div>
  </div>
</template>

<script>
import { Cloudinary } from 'cloudinary-core';

export default {
  data() {
    return {
      searchQuery: '',
      searchResults: [],
    };
  },
  methods: {
    async searchImages() {
      if (this.searchQuery.length < 3) {
        this.searchResults = [];
        return;
      }

      try {
        const response = await fetch(
          'https://cloudinary-search-worker.braddev.workers.dev/',
          {
            method: 'POST',
            headers: {
              'Content-Type': 'application/json',
            },
            body: JSON.stringify({ searchQuery: this.searchQuery }),
          },
        );

        if (!response.ok) {
          throw new Error(`HTTP error! Status: ${response.status}`);
        }

        const resources = await response.json();
        const cloudinary = new Cloudinary({
          cloud_name: 'brad-dev',
          secure: true,
        });
        this.searchResults = resources.map((resource) => ({
          url: cloudinary.url(resource.public_id),
          public_id: resource.public_id,
        }));
      } catch (error) {
        console.error('Error searching images:', error);
      }
    },
  },
};
</script>
