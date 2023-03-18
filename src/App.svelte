<script lang="ts">
  import { ImageStatus } from '../types.d'
  import { imageStatus } from './store'
  import StepUpload from './StepUpload.svelte'
  import StepEdit from './StepEdit.svelte'
  import Navbar from './components/Navbar.svelte'
</script>

<main
  class="px-10 pb-12 w-full min-h-screen flex flex-col items-center justify-start relative overflow-hidden"
>
  <Navbar />
  <h1 class="mb-6 text-center text-5xl font-semibold tracking-tight">
    Cleanbg
  </h1>
  <p
    class="mb-12 max-w-lg text-center text-base font-normal text-neutral-400 leading-snug"
  >
    Say goodbye to distracting backgrounds with our free and easy-to-use
    background remover app. Perfect for product photos and portraits.
  </p>
  <section class="w-full max-w-xl grid grid-cols-1 place-content-center">
    {#if $imageStatus === ImageStatus.READY || $imageStatus === ImageStatus.UPLOADING}
      <StepUpload />
    {:else if $imageStatus === ImageStatus.DONE}
      <StepEdit />
    {/if}
  </section>
  <div class="gradient_down" />
</main>

<style>
  @import url('https://fonts.googleapis.com/css2?family=Poppins:wght@100;200;300;400;500;600;700;800;900&display=swap');

  :global(body) {
    font-family: 'Poppins', sans-serif;
  }

  .gradient_down {
    z-index: -1;
    width: 200%;
    height: 100%;
    background: linear-gradient(83.21deg, #b721ff 0%, #c850c0 100%);
    background-blend-mode: overlay;
    -webkit-mask-image: radial-gradient(rgb(255, 255, 255), transparent 75%);
    mask-image: radial-gradient(rgba(255, 255, 255) transparent 75%);
    position: absolute;
    top: 60%;
  }
</style>
