<template>
  <div id="home">
    <div>Hello world from your Vue project. Below is Builder Content:</div>

    <div v-if="content || isPreviewing()">
      <div>
        page title:
        {{ content?.data?.title || 'Unpublished' }}
      </div>

      <!-- <label @click="activeComponent = CompA">
        <input type="radio" v-model="activeComponent" :value="CompA"> A
      </label>
      <label  @click="activeComponent = CompB">
        <input type="radio" v-model="activeComponent" :value="CompB"> B
      </label>
      <Transition name="fade" mode="out-in">
        <component :is="activeComponent"></component>
      </Transition> -->

      <Content
        model="page"
        :content="content"
        :api-key="BUILDER_PUBLIC_API_KEY"
        :customComponents="REGISTERED_COMPONENTS"
        :context="{ myFunction: handleButtonClick }"
        :data="{ products: products }"
      />
    </div>
    <div v-else>Content not Found</div>
  </div>
</template>

<script setup>
import { Content, register, fetchOneEntry, isPreviewing, getBuilderSearchParams } from '@builder.io/sdk-vue';

import HelloWorldComponent from './components/HelloWorld.vue';
import Tabs from './components/Tabs.vue';
import Header from './components/Header.vue';
import Footer from './components/Footer.vue';
import Megamenu from './components/Megamenu.vue';

import CompA from './components/CompA.vue';
import CompB from './components/CompB.vue';
import ProductCard from './components/ProductCard.vue';

let products = [
        {
          id: 1,
          name: "Laptop",
          price: 999.99,
          description: "A high-performance laptop for all your needs."
        },
        {
          id: 2,
          name: "Smartphone",
          price: 599.99,
          description: "A smartphone with the latest features and technology."
        },
        {
          id: 3,
          name: "Headphones",
          price: 199.99,
          description: "Noise-cancelling headphones for an immersive experience."
        }
        ];

// Method to be passed to the Content component
function handleButtonClick() {
  alert('Hi! I am a custom function! 🎉');
}

const defaultTab = {
  '@type': '@builder.io/sdk:Element',
  responsiveStyles: {
    large: {
      paddingLeft: '20px',
      paddingRight: '20px',
      paddingTop: '10px',
      paddingBottom: '10px',
      minWidth: '100px',
      textAlign: 'center',
      // TODO: add to all
      display: 'flex',
      flexDirection: 'column',
      cursor: 'pointer',
      userSelect: 'none',
    },
  },
  component: {
    // Builder:text
    name: 'Text',
    options: {
      text: 'New tab',
    },
  },
};

const defaultElement = {
  '@type': '@builder.io/sdk:Element',
  responsiveStyles: {
    large: {
      height: '200px',
      display: 'flex',
      marginTop: '20px',
      flexDirection: 'column',
    },
  },
  component: {
    name: 'Text',
    options: {
      text: 'New tab content ',
    },
  },
};

register('insertMenu', {
  name: 'Nav Specifics',
  items: [
    { name: 'Header' },
    { name: 'Footer' },
    { name: 'Megamenu' },
  ],
})

register("editor.settings", {
  strictMode: true, // optional
  designTokens: {
    colors: [
      { name: "Red", value: "rgba(255, 0, 0)" },
      { name: "Builder Blue", value: "rgba(93, 150, 255, 1)" },
    ],
    spacing: [
      { name: "Large", value: "var(--space-large, 20px)" },
      { name: "Small", value: "10px" },
      { name: "Tiny", value: "5px" },
    ],
    fontFamily: [
      { name: 'Serif', value: 'Times, serif' },
      { name: 'Sans serif', value: 'Roboto, sans-serif' }
    ]
  },
});

// Register your Builder components
const REGISTERED_COMPONENTS = [ 
{
    component: HelloWorldComponent,
    name: 'MyFunComponent',
    canHaveChildren: true,
    inputs: [
      {
        name: 'text',
        type: 'string',
        defaultValue: 'World',
      },
    ],
  },
  {
    component: ProductCard,
    name: 'MyProductCard',
    canHaveChildren: true,
    inputs: [
    {
      name: 'content',
      type: 'uiBlocks',
      hideFromUI: true,
      defaultValue: [],
    },
    ],
  },
  {
    component: Header,
    name: 'Header',
    inputs: [
    {
      name: 'brandName',
      type: 'string',
      required: true,
      helperText: 'The name of the brand'
    },
    {
      name: 'logo',
      type: 'file',
      required: true,
      helperText: 'The logo of the brand'
    }
  ]
  },
  {
    component: Footer,
    name: 'Footer',
  },
  {
    component: Megamenu,
    name: 'Megamenu',
    inputs: [
    {
      name: 'brandName',
      type: 'string',
      required: true,
      helperText: 'The name of the brand'
    },
    {
      name: 'logo',
      type: 'file',
      required: true,
      helperText: 'The logo of the brand'
    }
  ]
  },
  {
  component: Tabs,
  name: 'Tabs',
  canHaveChildren: true,
  inputs: [
    {
      name: 'tabs',
      type: 'list',
      broadcast: true,
      subFields: [
        {
          name: 'label',
          type: 'uiBlocks',
          hideFromUI: true,
          defaultValue: [defaultTab],
        },
        {
          name: 'content',
          type: 'uiBlocks',
          hideFromUI: true,
          defaultValue: [defaultElement],
        },
      ],
      defaultValue: [
        {
          label: [
            {
              ...defaultTab,
              component: {
                name: 'Text',
                options: {
                  text: 'Tab 1',
                },
              },
            },
          ],
          content: [
            {
              ...defaultElement,
              component: {
                name: 'Text',
                options: {
                  text: 'Tab 1 content',
                },
              },
            },
          ],
        },
        {
          label: [
            {
              ...defaultTab,
              component: {
                name: 'Text',
                options: {
                  text: 'Tab 2',
                },
              },
            },
          ],
          content: [
            {
              ...defaultElement,
              component: {
                name: 'Text',
                options: {
                  text: 'Tab 2 content',
                },
              },
            },
          ],
        },
      ],
    },
    {
      name: 'activeTabStyle',
      type: 'uiStyle',
      helperText: 'CSS styles for the active tab',
      defaultValue: {
        backgroundColor: 'rgba(0, 0, 0, 1)',
      },
    },
    {
      name: 'defaultActiveTab',
      type: 'number',
      helperText:
        'Default tab to open to. Set to "1" for the first tab, "2" for the second, or choose "0" for none',
      defaultValue: 1,
      advanced: true,
    },
    {
      name: 'collapsible',
      type: 'boolean',
      helperText: 'If on, clicking an open tab closes it so no tabs are active',
      defaultValue: false,
      advanced: true,
    },
    {
      name: 'tabHeaderLayout',
      type: 'enum',
      helperText: 'Change the layout of the tab headers (uses justify-content)',
      defaultValue: 'flex-start',
      enum: [
        { label: 'Center', value: 'center' },
        { label: 'Space between', value: 'space-between' },
        { label: 'Space around', value: 'space-around' },
        { label: 'Left', value: 'flex-start' },
        { label: 'Right', value: 'flex-end' },
      ],
    },
  ],
},
];

// TODO: enter your public API key
const BUILDER_PUBLIC_API_KEY = '6641374915bc47c1b0bcff83bbe6e797'; // ggignore

const route = useRoute();

// fetch builder content data
const { data: content } = await useAsyncData('builderData', () =>
  fetchOneEntry({
    model: 'page',
    apiKey: BUILDER_PUBLIC_API_KEY,
    userAttributes: {
      urlPath: route.path,
    },
    options: getBuilderSearchParams(route.query),
  })
);
</script>
<style>
.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.5s ease;
}

.fade-enter-from,
.fade-leave-to {
  opacity: 0;
}
</style>