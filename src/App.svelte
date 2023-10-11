<script lang="ts">
  import { ImageStatus } from '../types.d'
  import { imageStatus } from './store'
  import StepUpload from './StepUpload.svelte'
  import StepEdit from './StepEdit.svelte'
  import Navbar from './components/Navbar.svelte'
</script>

<main
  class="px-10 pb-12 w-full min-h-screen bg-[#131313] flex flex-col items-center justify-start relative overflow-hidden"
>
  <Navbar />
  <h1
    class="mb-6 text-white text-center text-5xl font-semibold tracking-tight xl:mb-12 xl:text-8xl"
  >
    Cleanbg
  </h1>
  <p
    class="mb-12 max-w-lg text-center text-base font-medium text-neutral-400 leading-snug xl:max-w-2xl xl:text-2xl"
  >
    Say goodbye to distracting backgrounds with our free and easy-to-use
    background remover app. Perfect for product photos and portraits.
  </p>
  <section
    class="w-full max-w-xl grid grid-cols-1 place-content-center xl:max-w-2xl"
  >
    {#if $imageStatus === ImageStatus.READY || $imageStatus === ImageStatus.UPLOADING}
      <StepUpload />
    {:else if $imageStatus === ImageStatus.DONE}
      <StepEdit />
    {/if}
  </section>
</main>

<style>
  @import url('https://fonts.googleapis.com/css2?family=Poppins:wght@100;200;300;400;500;600;700;800;900&display=swap');

  :global(body) {
    font-family: 'Poppins', sans-serif;
  }

  main::before {
    content: '';
    width: 240px;
    height: 240px;
    background: linear-gradient(
      125deg,
      rgb(100, 255, 227, 50%) 5%,
      rgb(100, 255, 227, 15%) 25%,
      rgb(100, 255, 227, 7.5%) 100%
    );
    filter: blur(80px);
    border-radius: 100%;
    position: absolute;
    top: 0;
    left: 0;
    overflow: hidden;
    z-index: 1;
  }

  @media only screen and (min-width: 1280px) {
    main::before {
      width: 620px;
      height: 620px;
      top: 15%;
      left: 15%;
    }
  }
</style>
