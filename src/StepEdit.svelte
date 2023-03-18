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

<two-up class="two-up">
  <img
    class="rounded-2xl shadow-2xl"
    src={$originalImage}
    alt="uploaded for user"
  />
  {#if processingImage}
    <img
      class="rounded-2xl shadow-2xl blur-sm	grayscale"
      src={$originalImage}
      alt="uploaded for user"
    />
  {:else}
    <img class="rounded-2xl shadow-2xl" src={$modifiedImage} alt="changed" />
  {/if}
</two-up>

<div class="mt-6 w-full flex items-center justify-center">
  <a
    download
    href={$modifiedImage}
    target="_blank"
    class="px-10 py-3 bg-black text-xl text-white text-medium rounded-full flex items-center justify-center gap-2"
  >
    Download
    <DownloadIcon />
  </a>
</div>

<style>
  .two-up {
    --accent-colour: #000000;
    --track-color: #000000;
    --thumb-color: #000000;
    --thumb-background: #fff;
    --thumb-size: 50px;
    --bar-size: 3px;
    --bar-touch-size: 25px;
  }
</style>
