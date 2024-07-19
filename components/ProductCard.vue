<template>
  <div class="card__container">
    <slot name="header">
      <div class="flex items-center justify-between">
        <span class="pill__tag">New</span>
        <span class="bordered__tag">20% off</span>
      </div>
    </slot>

    <NuxtLink to="#" class="my-4">
      <!-- NEED ADD BLOCK SLOTS WHILE STILL BE ABLE TO RENDER DEFAULT CONTENT -->
      <slot name="image">
        <!-- ADD BLOCK TO OVERRIDE DEFAULT IMAGE AND DRAG & DROP ANYTHING IN HERE -->
        <Blocks
          class="mt-4"
          :parent="props.builderBlock?.id"
          :path="`component.options.image`"
          :blocks="props.content"
        />
      </slot>
    </NuxtLink>

    <div class="text-center mt-2">
      <NuxtLink to="#">
        <slot name="title">
          <p class="product__title">
            {{ title }}
          </p>
        </slot>
      </NuxtLink>
      <div class="flex items-center mt-2.5 mb-5 justify-center">
        <div class="flex items-center space-x-1 rtl:space-x-reverse">
          <Icon
            v-for="n in rating"
            :key="n"
            name="material-symbols:star"
            class="text-yellow-300"
            aria-hidden="true"
          />
        </div>
        <span class="pill__tag ms-3">{{ rating }}.0</span>
      </div>
      <div class="flex flex-col gap-2">
        <span class="text-3xl font-bold text-gray-900 dark:text-white"
          >${{ price }}</span
        >
        <slot name="footer">
          <button type="button" class="btn">Add To Trolley</button>
        </slot>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import { Blocks } from "@builder.io/sdk-vue";
import type { PropType } from "vue";

type BuilderBlock = {
  id: string | number;
  [key: string]: any;
};

type BuilderContent = Record<string, any>[];

const props = defineProps({
  title: {
    type: String,
    required: true,
  },
  rating: Number,
  price: Number,
  builderBlock: Object as PropType<BuilderBlock>,
  content: { type: Array as PropType<BuilderContent>, default: () => [] },
});
</script>

<style scoped>
.card__container {
  @apply w-full max-w-[350px] bg-white rounded-lg border border-gray-200 shadow hover:shadow-lg transition-all p-5 dark:bg-gray-800 dark:border-gray-700;
}

.pill__tag {
  @apply bg-blue-100 text-blue-800 text-sm font-medium me-2 px-2.5 py-1 rounded-full dark:bg-blue-900 dark:text-blue-300;
}

.bordered__tag {
  @apply bg-purple-100 text-purple-800 text-sm font-medium me-2 px-2.5 py-1 rounded-full dark:bg-gray-700 dark:text-purple-400 border border-purple-400;
}

.product__title {
  @apply text-xl font-semibold tracking-tight text-gray-900 dark:text-white;
}

.btn {
  @apply text-white bg-blue-700 hover:bg-blue-800 focus:ring-4 focus:outline-none focus:ring-blue-300 font-medium rounded-lg text-sm px-5 py-2.5 text-center dark:bg-blue-600 dark:hover:bg-blue-700 dark:focus:ring-blue-800;
}
</style>
