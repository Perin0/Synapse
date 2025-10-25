<script lang="ts">
  let name = '';
  let message = '';
  const submit = (e: Event) => { e.preventDefault(); alert('Mensagem enviada (simulação)'); }
</script>

<section class="institutional-page">
  <h1>Contato</h1>
  <form class="contact-form" on:submit|preventDefault={submit}>
    <label for="contact-name">Nome</label>
    <input id="contact-name" bind:value={name} />
    <label for="contact-message">Mensagem</label>
    <textarea id="contact-message" bind:value={message}></textarea>
    <button type="submit" class="btn btn-primary">Enviar</button>
  </form>
</section>
