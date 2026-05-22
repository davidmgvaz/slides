<template>
  <footer class="isla-foot" v-if="$slidev.nav.currentPage > 1">
    <span class="fl">Django 5.2 &middot; ISLA Gaia &middot; Programa&ccedil;&atilde;o de Bases de Dados</span>
    <span class="fr">{{ $slidev.nav.currentPage }} / {{ $slidev.nav.total }}</span>
  </footer>
</template>
