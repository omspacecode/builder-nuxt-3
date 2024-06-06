<template>
    <div class="my-4">
      <!-- NEED ADD BLOCK FOR NAMED SLOTS WITH DEFAULT CONTENT -->
      <slot name="image">
        <!-- ADD BLOCK TO OVERRIDE DEFAULT IMAGE AND DRAG & DROP ANYTHING IN HERE -->
        <Blocks
          class="mt-4"
          :parent="props.builderBlock?.id"
          :path="`component.options.content`"
          :blocks="props.content"
        />

        <NuxtImg
          :src="image"
          alt="product image"
          class="p-8 rounded-t-lg"
        />
      </slot>
    </div>
</template>

<script setup>
import { Blocks } from '@builder.io/sdk-vue';
  
  const props = defineProps({
    builderBlock: Object,
    content: Array,
    image: String,
    //imageUrl: String,
  });
</script>


<!-- <template>
    <div class="builder-card">
      <div class="builder-card-content">
        <slot name="image">
            <Blocks
            :parent="props.builderBlock.id"
            :path="`component.options.content`"
            :blocks="props.content"
            />
        </slot>
      </div>
    </div>
  </template>
  
  <script setup>
  import { Blocks } from '@builder.io/sdk-vue';
  
  const props = defineProps({
    builderBlock: Object,
    content: Array,
  });
  </script>
  
  <style scoped>
  .builder-card {
    border: 1px solid #ccc;
    border-radius: 4px;
    overflow: hidden;
    background-color: #fff;
    box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
    transition: box-shadow 0.2s;
  }
  
  .builder-card:hover {
    box-shadow: 0 4px 8px rgba(0, 0, 0, 0.15);
  }
  
  .builder-card-image {
    width: 100%;
    height: auto;
    display: block;
    overflow: hidden;
  }
  
  .builder-card-content {
    padding: 15px;
  }
  </style>
   -->