<template>
    <div class="super-product-card">
      <div class="product-image-wrapper">
        <Blocks
          :parent="props.builderBlock.id"
          :path="`component.options.productImage`"
          :blocks="productImage"
        />
      </div>
      <div class="product-info">
        <Blocks
          :parent="props.builderBlock.id"
          :path="`component.options.productName`"
          :blocks="productName"
        />
        <Blocks
          :parent="props.builderBlock.id"
          :path="`component.options.productDescription`"
          :blocks="productDescription"
        />
        <Blocks
          :parent="props.builderBlock.id"
          :path="`component.options.productPrice`"
          :blocks="productPrice"
        />
      </div>
    </div>
  </template>
  
  <script setup>
  import { Blocks } from '@builder.io/sdk-vue';
  const props = defineProps({
    builderBlock: Object,
    productImage: Array,
    productName: Array,
    productDescription: Array,
    productPrice: Array,
  });
  </script>
  
  <style scoped>
  .super-product-card {
    border: 1px solid #ccc;
    border-radius: 8px;
    overflow: hidden;
    display: flex;
    flex-direction: column;
    align-items: center;
    background-color: #fff;
    box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
  }
  
  .product-image-wrapper {
    width: 100%;
  }
  
  .product-image-wrapper img {
    width: 100%;
    height: auto;
  }
  
  .product-info {
    padding: 16px;
    text-align: center;
  }
  
  .product-info > * {
    margin-bottom: 8px;
  }
  </style>
  
<!-- <template>
    <div class="super-product-card">
      <Blocks
        :parent="props.builderBlock.id"
        :path="`component.options.productImage`"
        :blocks="productImage"
      />
      <div class="product-info">
        <Blocks
          :parent="props.builderBlock.id"
          :path="`component.options.productName`"
          :blocks="productName"
        />
        <Blocks
          :parent="props.builderBlock.id"
          :path="`component.options.productDescription`"
          :blocks="productDescription"
        />
        <Blocks
          :parent="props.builderBlock.id"
          :path="`component.options.productPrice`"
          :blocks="productPrice"
        />
      </div>
    </div>
  </template>
  
  <script setup>
  import { Blocks } from '@builder.io/sdk-vue';
  const props = defineProps({
    builderBlock: Object,
    productImage: Array,
    productName: Array,
    productDescription: Array,
    productPrice: Array,
  });
  </script>
  
  <style scoped>
  .super-product-card {
    border: 1px solid #ccc;
    border-radius: 8px;
    overflow: hidden;
    display: flex;
    flex-direction: column;
    align-items: center;
    background-color: #fff;
    box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
  }
  
  .product-info {
    padding: 16px;
    text-align: center;
  }
  
  .product-info > * {
    margin-bottom: 8px;
  }
  </style>
   -->