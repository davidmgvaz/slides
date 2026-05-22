<script setup>
import logo from './isla-logo.png'
</script>

<template>
  <div class="isla-corner-logo"><img :src="logo" alt="ISLA" /></div>
  <footer class="isla-foot" v-if="$slidev.nav.currentPage > 1">
    <span class="fl">Django &middot; Databases &middot; LICE</span>
    <span class="fr">{{ $slidev.nav.currentPage }} / {{ $slidev.nav.total }}</span>
  </footer>
</template>
