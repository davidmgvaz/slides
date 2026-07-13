<!-- Persistent footer strip — Patch Notes (1b).
     Copy next to slides.md; Slidev auto-loads global-bottom.vue on every slide.
     Hidden on intro + section slides (they have their own furniture). -->
<template>
  <footer
    v-if="!hidden"
    class="ep-global-footer"
    :class="{ 'no-rule': isSection }"
  >
    <div v-if="!isSection" class="hatch"></div>
    <span>david vaz · <em>@davidmgvaz</em></span>
    <span class="right">
      <span>europython 2026</span>
      <img src="./images/boost-logo-dark.png" alt="Boost">
    </span>
  </footer>
</template>

<script setup>
import { computed } from 'vue'
import { useSlideContext } from '@slidev/client'

const { $slidev } = useSlideContext()
const meta = computed(() => $slidev.nav.currentSlideRoute?.meta?.slide?.frontmatter ?? {})
const isSection = computed(() => meta.value.layout === 'section')
const hidden = computed(() => {
  return meta.value.layout === 'intro'
    || meta.value.hideFooter === true // per-slide opt-out: `hideFooter: true`
})
</script>

<style scoped>
.ep-global-footer {
  position: absolute;
  left: 3.8rem;
  right: 3.8rem;
  bottom: 1.1rem;
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding-top: 0.6rem;
  font-family: 'JetBrains Mono', monospace;
  font-size: 0.62rem;
  color: rgba(18, 48, 31, 0.45);
  pointer-events: none;
  z-index: 10;
}
.ep-global-footer.no-rule { padding-top: 0; } /* section: full hatch band is the divider */
/* mini hatch strip — the section-slide stripes, shrunk. Own element with a
   view-transition-name so it morphs into the big section band when the deck
   uses `transition: view-transition` (see README). */
.ep-global-footer .hatch {
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  height: 0.28rem;
  background: repeating-linear-gradient(-45deg, rgba(46, 148, 99, 0.55) 0 2.5px, transparent 2.5px 9px);
  view-transition-name: ep-hatch;
}
.ep-global-footer em { color: #2E9463; font-style: normal; }
.ep-global-footer .right { display: flex; align-items: baseline; gap: 0.7rem; }
/* "B" (full image height) matched to JetBrains Mono cap/digit height (0.73em),
   baseline-aligned — so the logo caps line up exactly with "2026" */
.ep-global-footer img { height: 0.85em; width: auto; opacity: 0.55; }
</style>
