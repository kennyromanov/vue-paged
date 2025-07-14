<script lang="ts">

import { defineComponent, ref } from 'vue';
import { Previewer } from 'pagedjs';
import { BaseError } from '@/errors';

export default defineComponent({
  name: 'VuePaged',

  setup() {
    const displayEl = ref<any>(null);
    const contentEl = ref<any>(null);

    return { displayEl, contentEl };
  },

  methods: {
    async updHtml(): Promise<void> {

      // Doing some checks

      if (!this.displayEl || !this.contentEl)
        throw new BaseError('Vue Paged: Unable to render content, the component is not ready');


      // Clearing the previous render

      this.remHtml();


      // Updating the HTML

      await new Previewer()?.preview(
          this.contentEl?.innerHTML,
          [],
          this.displayEl,
      );
    },
    remHtml(): void {
      this.displayEl.innerHTML = '';
    },
  },

  watch: {
    displayEl: {
      handler(val: any): void {

        // Doing some checks

        if (!val || !this.contentEl) return;


        // TODO: kr: Costyl to ensure all the previous tasks are done

        const upd = () => {
          this.updHtml().then(() => {
            this.$emit('compiled');
          }).catch(console.error);
        };

        this.$emit('compilation:started');

        setTimeout(upd, 0);
      },
      immediate: true
    },
  },
});

</script>

<template>
  <div class="vue_paged">
    <div class="vue_paged_inner" ref="displayEl" />

    <div class="vue_paged_content hidden" ref="contentEl">
      <slot>
        <span />
      </slot>
    </div>
  </div>
</template>