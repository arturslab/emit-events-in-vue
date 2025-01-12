<template>
  <div class="card">
    <h2>Child Component {{ title }}</h2>
    <h3>reload: <span :class="reload ? 'value_true' : 'value_false'">{{ reload }}</span></h3>
    <p class="bg_red" v-if="load">
      Reload component {{ title }} in progress...
    </p>
    <p class="bg_green" v-else>Child {{ title }} content</p>

    <div class="form-check">
      <input type="checkbox" class="form-check-input" :id="`useRoot${childIndex}`" v-model="useRoot">
      <label class="form-check-label" :for="`useRoot${childIndex}`"> use $root</label>
    </div>

    <div class="buttons">
      <button class="btn" @click="onReload('One')">Reload One</button>
      <button class="btn" @click="onReload('Two')">Reload Two</button>
      <button :class="['btn', { loading: load }]" :disabled="load" @click="onReload(childIndex)">
        Reload Three
      </button>
    </div>

    <div class="mt-10" v-if="useRoot">
      It will emit event using $root:
      <code>
        this.$root.$emit("onComponentChange", {
          reload: true,
          target: '{{ imageConfig.targetChild }}'
        });
      </code>
    </div>
  </div>
</template>

<script>
export default {
  name: "ComponentThree",
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
      childIndex: 'Three',
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
