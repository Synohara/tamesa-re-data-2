<template>
  <footer class="footer">
    <span class="footer-page">{{ $nav.currentPage }}</span>
  </footer>
</template>

<style scoped>
.footer {
  position: fixed;
  bottom: 0;
  right: 0;
  padding: 12px 32px;
  font-size: 0.75rem;
  color: #999;
  z-index: 100;
  pointer-events: none;
}
</style>
