<script>
  let secret = "";
  let message;
  console.log(secret.length);

  function addSecret() {
    localStorage.setItem("secret", CryptoJS.SHA256(secret).toString());
  }

  function removeSecret() {
    localStorage.removeItem("secret");
  }

  function encrypt() {
    const encrypted = CryptoJS.AES.encrypt(message, localStorage.getItem("secret"));
    message = encrypted.toString();
  }

  function decrypt() {
    const decrypted = CryptoJS.AES.decrypt(message, localStorage.getItem("secret"));
    message = decrypted.toString(CryptoJS.enc.Utf8);
  }

  function copy() {
    navigator.clipboard.writeText(message);
  }

  function paste() {
    navigator.clipboard.readText().then(clipText => {
      message = clipText;
    });
  }

  function clear() {
    message = "";
  }

</script>

<main>
  <h1>Encrypted Messages</h1>
  {#if !localStorage.getItem("secret")}
    <form on:submit="{addSecret}" class="add">
      <input type="text" bind:value={secret} placeholder="Introduce tú frase secreta">
      <input type="submit" value="Añadir frase" disabled="{secret.length < 3}">
    </form>
  {/if}
  <form on:submit={removeSecret} class="remove">
    {#if localStorage.getItem("secret")}
      <span class="yes">¡Frase añadida!</span>
      <input type="submit" value="Borrar frase">
    {:else}
      <span class="no">¡No hay frase añadida!</span>
    {/if}
  </form>
  {#if localStorage.getItem("secret")}
    <div class="message">
      <textarea bind:value={message} placeholder="Introduce tú mensaje"></textarea>
      <button on:click={encrypt}>Encriptar</button>
      <button on:click={decrypt}>Desencriptar</button>
      <button on:click={copy}>Copiar</button>
      <button on:click={paste}>Pegar</button>
      <button on:click={clear}>Limpiar</button>
    </div>
  {/if}
</main>

<style>
  :root {
    --btn-bg: #2196f3;
    --btn-color: #fff;
    --red: #f44336;
    --green: #4caf50;
  }

  h1 {
    text-align: center;
    padding: 10px 0;
  }

  .add {
    display: flex;
    flex-direction: column;
    align-items: center;
  }

  input[type="text"] {
    width: 80%;
    padding: 5px 10px;
    margin: 5px 0;
    font-family: inherit;
    border: 1px solid #ccc;
    border-radius: 5px;
  }

  input[type="submit"], button {
    width: 80%;
    padding: 5px 10px;
    margin: 5px 0;
    font-family: inherit;
    border: 1px solid var(--btn-bg);
    border-radius: 5px;
    background-color: var(--btn-bg);
    color: var(--btn-color);
    cursor: pointer;
  }

  input[type="submit"]:disabled {
    background-color: #ccc;
    color: #666;
    cursor: not-allowed;
    border: 1px solid #ccc;
  }

  .remove {
    display: flex;
    flex-direction: column;
    align-items: center;
  }

  .remove .no {
    color: var(--red);
  }

  .remove .yes {
    color: var(--green);
  }

  .remove span {
    margin: 0 0 5px;
  }

  .message {
    display: flex;
    flex-direction: column;
    align-items: center;
  }

  textarea {
    width: 80%;
    height: 200px;
    padding: 10px 10px;
    margin: 5px 0;
    font-family: inherit;
    border: 1px solid #ccc;
    border-radius: 5px;
  }
</style>
