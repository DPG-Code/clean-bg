<script lang="ts">
  import 'two-up-element'
  import DownloadIcon from './components/DownloadIcon.svelte'
  import { originalImage, modifiedImage } from './store'

  let processingImage = true
  let tries = 0
  let intervalId: any

  $: {
    if (processingImage) {
      clearInterval(intervalId)
      intervalId = setInterval(() => {
        tries++
        const img = new Image()
        img.src = $modifiedImage
        img.onload = () => {
          processingImage = false
          clearInterval(intervalId)
        }
      }, 500)
    }
  }
</script>

<two-up class="two-up z-40">
  <img
    class="rounded-2xl shadow-2xl xl:rounded-3xl"
    src={$originalImage}
    alt="uploaded for user"
  />
  {#if processingImage}
    <img
      class="rounded-2xl shadow-2xl blur-sm grayscale xl:rounded-3xl"
      src={$originalImage}
      alt="uploaded for user"
    />
  {:else}
    <img
      class="rounded-2xl shadow-2xl xl:rounded-3xl"
      src={$modifiedImage}
      alt="changed"
    />
  {/if}
</two-up>

<div class="mt-6 w-full flex items-center justify-center xl:mt-10">
  <a
    download
    href={$modifiedImage}
    target="_blank"
    class="px-10 py-3 bg-[#64ffe360] text-xl text-white text-medium rounded-full flex items-center justify-center gap-2 xl:px-16 xl:py-4 xl:text-2xl xl:gap-6"
  >
    Download
    <DownloadIcon />
  </a>
</div>

<style>
  .two-up {
    --accent-colour: #131313;
    --track-color: #131313;
    --thumb-color: #131313;
    --thumb-background: #ffffff;
    --thumb-size: 50px;
    --bar-size: 3px;
    --bar-touch-size: 25px;
  }
</style>
