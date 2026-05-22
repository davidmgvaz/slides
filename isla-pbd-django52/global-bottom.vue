<script setup>
import logo from './isla-logo.png'
</script>

<template>
  <div class="isla-corner-logo"><img :src="logo" alt="ISLA" /></div>
  <footer class="isla-foot" v-if="$slidev.nav.currentPage > 1">
    <span class="fl">Django &middot; Programa&ccedil;&atilde;o de Bases de Dados &middot; LEI</span>
    <span class="fr">{{ $slidev.nav.currentPage }} / {{ $slidev.nav.total }}</span>
  </footer>
</template>
