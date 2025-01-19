<template>
  <div class="mve-card">
    <h2>Child Component {{ title }}</h2>
    <h3>reload: <span :class="mve - reload ? 'mve-value_true' : 'mve-value_false'">{{ reload }}</span></h3>
    <p class="mve-bg_red" v-if="load">
      Reload component {{ title }} in progress...
    </p>
    <p class="mve-bg_green" v-else>Child {{ title }} content</p>

    <div class="mve-form-check">
      <input type="checkbox" class="mve-form-check-input" :id="`useRoot${childIndex}`" v-model="useRoot">
      <label class="mve-form-check-label" :for="`useRoot${childIndex}`"> use $root</label>
    </div>

    <div class="mve-buttons">
      <button :class="['mve-btn', { 'mve-loading': load }]" :disabled="load" @click="onReload(childIndex)">
        Reload One
      </button>
      <button class="mve-btn" @click="onReload('Two')">Reload Two</button>
      <button class="mve-btn" @click="onReload('Three')">Reload Three</button>
    </div>

    <div class="mve-mt-10" v-if="useRoot">
      It will emit event using $root:
      <div class="mve-code">
        this.$root.$emit("onComponentChange", {
        reload: true,
        target: '{{ imageConfig.targetChild }}'
        });
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "ComponentOne",
  props: {
    title: {
      type: String,
      required: true,
    },
    reload: Boolean,
  },
  data() {
    return {
      load: false,
      useRoot: true,
      childIndex: 'One',
      imageConfig: {
        firstChild: "",
        targetChild: "",
        useRoot: true,
      },
    };
  },
  mounted() {
    this.$root.$on('onComponentChange', data => {
      if (data.target === this.childIndex) {
        this.componentMethod();
      }

    });
  },
  methods: {
    onReload(index) {

      this.imageConfig = {
        firstChild: this.childIndex,
        targetChild: index,
        useRoot: this.useRoot,
      };
      this.$emit("onImageChange", this.imageConfig);


      if (this.useRoot) {
        this.$root.$emit("onComponentChange", {
          reload: true,
          target: index,
        });
        return;
      }

      this.$emit("onComponentChange", {
        reload: true,
        target: index,
      });

    },
    componentMethod() {
      this.load = true;
      setTimeout(() => {
        this.load = false;
      }, 2000);
    },
  },
  watch: {
    reload() {
      this.componentMethod();
    },
  },
};
</script>
