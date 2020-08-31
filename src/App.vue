<template>
  <div id="main" class="flex flex-col items-center justify-center w-full mt-12 mb-12">
    <div class="flex w-full">
      <div class="flex w-1/3 flex-col">
        <div class="p-6 mb-6 rounded-r-lg shadow-lg" :style="{backgroundColor: color}" v-for="({name, description, color, tags}, index) in categories" :key=index>
          <label :for=category class="flex flex-col cursor-pointer">
            <div class="flex w-full justify-between">
              <div class="text-gray-100 font-bold text-lg">
                {{name}}
              </div>
              
              <div class="ml-3 relative">
                <input :id=category type="checkbox" checked class="hidden" @change="isToggled[index] = !isToggled[index]"/>
                <div class="toggle__line w-10 h-4 bg-cus rounded-full shadow-inner"></div>
                <div class="toggle__dot absolute w-6 h-6 bg-white rounded-full shadow inset-y-0 left-0"></div>
              </div>
            </div>

            <div class="text-gray-100 font-medium">
              {{ description }}
            </div>

          </label>

            <div class="pt-2">
              <span v-for="tag in tags" :key="tag" class="op inline-block bg-cus rounded-full px-3 py-1 text-xs font-semibold text-gray-700 mr-2 mb-2">
                {{labels[tag]}} ✔️
              </span>
            </div>

        </div>
      </div>

      <div class="w-2/3 ml-12 mr-12 grid grid-cols-3 grid-rows-5 gap-3">
        <div class="bg-white rounded-lg shadow-lg" v-for="({name, description, category}, index) in filteredEntries" :key="index">
          <div class="max-w-sm overflow-hidden">
            
            <div :style="{backgroundColor: categories[category[0]].color}" class="px-3 py-2 rounded-t-lg">
              <div class="font-bold text-xl text-gray-100 mb-2 mr-2">
                {{name}}
              </div>
            </div>

            <div class="px-3 py-2 rounded-t-lg">
              <p class="text-gray-700 text-base">
                {{description}}
              </p>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import { ref, computed } from 'vue'
import dataset from './data'

export default {
  setup() {
    const isToggled = ref(dataset.categories.map(x => true));
    const filters = computed(() => isToggled.value.reduce((acc, curr, i) => curr ? [...acc, i] : acc, []))
    const filteredEntries = computed(() => 
      dataset.entries.filter(entry => 
        filters.value.some(filter => entry.category.some(cat => cat === filter))
      ).sort(() => Math.random() - 0.5)
    )

    return {
      isToggled,
      categories: dataset.categories,
      filteredEntries,
      labels: dataset.tags
    }
  }
}
</script>

<style>
@tailwind base;
@tailwind components;
@tailwind utilities;

.toggle__dot {
  top: -.25rem;
  left: -.25rem;
  transition: all 0.3s ease-in-out;
}

input:checked ~ .toggle__dot {
  transform: translateX(100%);
  background-color: #20bf55;
}

.bg-cus {
  background-color: #fcfcfc;
}

html {
  background-color: #fcfcfc;
}
</style>
