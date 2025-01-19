<template>
  <div class="mve-vue-container">

    <div class="mve-row">
      <div class="mve-col">
        <component-intro />
      </div>
    </div>

    <div class="mve-card">

      <div class="mve-row">
        <div class="mve-col">
          <h2>Parent Component</h2>

          <br />items:
          <pre>{{ tabItems }}</pre>
        </div>
        <div class="mve-col">
          <demo-image :firstChild="imageConfig.firstChild" :secondChild="imageConfig.targetChild"
            :useRoot="imageConfig.useRoot" />
        </div>
      </div>

      <div class="mve-flex-row">
        <div class="mve-flex-col mve-tab-item" v-for="item in tabItems" :key="`tab-item-${item.componentName}`">
          <component :is="item.componentName" :title="item.title" :reload="item.reload"
            @onComponentChange="onComponentChange($event)" @onImageChange="onImageChange" />
        </div>
      </div>

    </div>

  </div>
</template>

<script>
import DemoImage from "./DemoImage.vue";
import ComponentIntro from "./ComponentIntro.vue";
import ComponentOne from "./ComponentOne.vue";
import ComponentTwo from "./ComponentTwo.vue";
import ComponentThree from "./ComponentThree.vue";

export default {
  name: "Home",
  components: { DemoImage, ComponentIntro, ComponentOne, ComponentTwo, ComponentThree },
  data() {
    return {
      tabItems: [
        {
          componentName: "ComponentOne",
          title: "One",
          reload: false,
        },
        {
          componentName: "ComponentTwo",
          title: "Two",
          reload: false,
        },
        {
          componentName: "ComponentThree",
          title: "Three",
          reload: false,
        },
      ],
      imageConfig: {
        firstChild: "",
        targetChild: "",
        useRoot: true,
      },
    };
  },
  methods: {
    onComponentChange({ reload, target }) {
      const index = this.tabItems.findIndex(
        (item) => item.title === target
      );
      this.tabItems[index].reload = !this.tabItems[index].reload;
    },
    onImageChange(imageConfig) {
      console.log(imageConfig);
      this.imageConfig = imageConfig;
    },
  },
};
</script>
