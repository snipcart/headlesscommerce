<template>
<div class="divide-y-1 divide-gray-500 divide-solid">
    <div id="main" class="flex flex-col items-center justify-center w-full mt-8">
      <span class="font-semibold text-2xl tracking-tight text-gray-700 mb-8">Headless & coupled e-commerce solutions</span>
      
      <div class="flex w-full">
        <div class="flex w-1/3 flex-col">
          <div class="p-6 mb-6 rounded-r-lg shadow-lg" :style="{backgroundColor: color}" v-for="({name, description, color, tags}, index) in categories" :key=index>
            <label :for=name class="flex flex-col cursor-pointer">
              <div class="flex w-full justify-between">
                <div class="text-gray-100 font-bold text-lg">
                  {{name}}
                </div>
                
                <div class="ml-3 relative">
                  <input :id=name type="checkbox" checked class="hidden" @change="isToggled[index] = !isToggled[index]"/>
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
          <div class="bg-white rounded-lg shadow-lg" v-for="({name, description, cat}) in filteredEntries" :key=name>
            <div>
              <div :style="getColorForCat(cat)" class="px-3 py-2 rounded-t-lg">
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

  <footer class="flex justify-around bg-cus w-full text-large text-gray-500 mt-8 p-5">
    <span>Created by the friendly bunch of nerds at <a class="underline font-semibold" href="https://snipcart.com/">Snipcart</a></span>
    <span class="underline font-semibold"><a href="https://github.com/snipcart/headlesscommerce">How to contribute?</a></span>
    <span class="underline font-semibold"><a href="https://snipcart.com/blog/tailwind-vue-tutorial">How we built it!</a></span>
    <span class="underline font-semibold"><a href="https://snipcart.com/blog/headless-ecommerce-guide">What is headless e-commerce?</a></span>
  </footer>
</div>
</template>

<script>
import { ref, computed } from 'vue'
import dataset from './data'

export default {
  setup() {
    const categories = dataset.categories;
    const isToggled = ref(dataset.categories.map(x => true));
    
    const filters = computed(() => 
      isToggled.value
        .reduce((acc, curr, i) => curr ? [...acc, i] : acc, [])
    )

    const filteredEntries = computed(() => 
      dataset.entries
        .filter(entry => filters.value
          .some(filter => entry.cat.some(cat => cat === filter)))
        .sort(() => Math.random() - 0.5)
    )

    const getColorForCat = (cat) => {
      return cat.length === 1
        ? { backgroundColor: categories[cat[0]].color }
        : { background: `linear-gradient(110deg, ${categories[cat[0]].color} 60%, ${categories[cat[1]].color} 60%)` }
    }

    return {
      isToggled,
      categories,
      filteredEntries,
      labels: dataset.tags,
      getColorForCat
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
