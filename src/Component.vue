<script lang="ts">

import { defineComponent, ref } from 'vue';
import { Previewer } from 'pagedjs';


// Third-parties

export const pagedjs = new Previewer();


export default defineComponent({
  name: 'VuePaged',

  setup() {
    const displayEl = ref<any>(null);
    const contentEl = ref<any>(null);

    return { displayEl, contentEl };
  },

  methods: {
    updHtml(): void {
      this.displayEl.innerHTML = '';

      pagedjs?.preview(
          this.contentEl.innerHTML,
          [],
          this.displayEl,
      );
    },
  },

  watch: {
    displayEl: {
      handler(val: string|null): void {
        if (!val || !this.contentEl) return;

        // TODO: kr: Costyl to ensure all the previous tasks are done
        setTimeout(this.updHtml.bind(this), 0);
      },
      immediate: true
    },
    // contentEl: {
    //   handler(val: string|null): void {
    //     if (!val || !this.displayEl) return;
    //
    //     // TODO: kr: Costyl to ensure all the previous tasks are done
    //     setTimeout(this.updHtml.bind(this), 0);
    //   },
    //   immediate: true
    // },
  },
});

</script>

<template>
  <div class="vue_paged">
    <div class="vue_paged_inner" ref="displayEl" />

    <div class="vue_paged_content hidden" ref="contentEl">
      <slot />
    </div>
  </div>
</template>