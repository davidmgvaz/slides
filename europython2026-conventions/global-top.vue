<!-- Presenter pacing clock — replaces the old no-op penguin leftover.
     Visible ONLY in presenter view (top-right of the slide preview).
     Elapsed vs the planned window for the current slide (from the clock
     stamps in the speaker notes — REGENERATE the PLAN array if timings
     change; see HANDOFF §7).
     Timer starts automatically when you first leave slide 1; click it to
     restart from zero. Green = ahead, ink = on plan, red = behind. -->
<template>
  <div
    v-if="isPresenter"
    class="ep-pacer"
    :class="state"
    @click="reset"
  >
    <span class="big">{{ fmt(elapsed) }}</span>
    <span class="win">plan {{ fmt(win[0]) }}–{{ fmt(win[1]) }}</span>
    <span class="delta">{{ delta >= 0 ? '+' : '−' }}{{ fmt(Math.abs(delta)) }}</span>
  </div>
</template>

<script setup>
import { computed, onMounted, onUnmounted, ref, watch } from 'vue'
import { useNav } from '@slidev/client'

const PLAN = [0, 30, 90, 110, 155, 185, 200, 260, 320, 335, 425, 500, 590, 665, 725, 755, 770, 815, 905, 995, 1085, 1100, 1110, 1120, 1130, 1140, 1215, 1275, 1290, 1380, 1455, 1515, 1525, 1585, 1585, 1660, 1705, 1735] // arrival seconds per page (1-based), last = total

const nav = useNav()
const isPresenter = computed(() => {
  const v = nav.isPresenter
  return typeof v === 'object' && v !== null ? !!v.value : !!v
})

const t0 = ref(null)
const now = ref(Date.now())
let iv
onMounted(() => { iv = setInterval(() => { now.value = Date.now() }, 1000) })
onUnmounted(() => clearInterval(iv))

const page = computed(() => {
  const v = nav.currentPage
  return typeof v === 'object' && v !== null ? v.value : v
})
watch(page, (p) => { if (p >= 2 && t0.value === null) t0.value = Date.now() })

const elapsed = computed(() => t0.value ? Math.floor((now.value - t0.value) / 1000) : 0)
const win = computed(() => {
  const i = Math.min(Math.max(page.value, 1), PLAN.length - 1)
  return [PLAN[i - 1], PLAN[i]]
})
const delta = computed(() => elapsed.value - win.value[0])
const state = computed(() =>
  elapsed.value > win.value[1] ? 'behind'
  : elapsed.value < win.value[0] ? 'ahead'
  : 'on')
function reset() { t0.value = Date.now() }
const fmt = s => `${Math.floor(s / 60)}:${String(s % 60).padStart(2, '0')}`
</script>

<style scoped>
.ep-pacer {
  position: absolute;
  top: 0.6rem;
  right: 0.8rem;
  z-index: 30;
  display: flex;
  align-items: baseline;
  gap: 0.55rem;
  padding: 0.3rem 0.6rem;
  border-radius: 8px;
  background: rgba(250, 248, 243, 0.92);
  border: 2px dashed rgba(18, 48, 31, 0.25);
  font-family: 'JetBrains Mono', monospace;
  font-size: 0.7rem;
  color: #12301F;
  cursor: pointer;
  user-select: none;
}
.ep-pacer .big { font-size: 1rem; font-weight: 700; }
.ep-pacer .win { opacity: 0.55; }
.ep-pacer.ahead .delta { color: #2E9463; font-weight: 700; }
.ep-pacer.on .delta { opacity: 0.7; }
.ep-pacer.behind { border-color: #A2452F; }
.ep-pacer.behind .delta { color: #A2452F; font-weight: 700; }
</style>
