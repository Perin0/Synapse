<script lang="ts">
  // Footer component ported from React footer
  export let year: number = new Date().getFullYear();
</script>

<footer class="footer">
  <div class="footer-content">
    <div class="footer-brand">
      <a href="/"> <img src="/logo.png" alt="MentorOn"/> </a>
      <p>Conectando pessoas e conhecimento.</p>
    </div>
    <div class="footer-links">
      <a href="/sobre">Sobre</a>
      <a href="/como-funciona">Como funciona</a>
      <a href="/contato">Contato</a>
      <a href="/politica-privacidade">Política de Privacidade</a>
    </div>
  </div>
  <div class="footer-bottom">© {year} MentorOn. Todos os direitos reservados.</div>
</footer>
