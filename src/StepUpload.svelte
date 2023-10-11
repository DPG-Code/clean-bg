<script lang="ts">
  import { Cloudinary } from '@cloudinary/url-gen'
  import { backgroundRemoval } from '@cloudinary/url-gen/actions/effect'

  import { imageStatus, modifiedImage, originalImage } from './store'
  import { ImageStatus } from '../types.d'
  import Dropzone from 'dropzone'
  import 'dropzone/dist/dropzone.css'

  import { onMount } from 'svelte'
  import Loader from './components/Loader.svelte'
  import UploadIcon from './components/UploadIcon.svelte'

  const cloudinary = new Cloudinary({
    cloud: {
      cloudName: 'dkk9jnnus'
    },
    url: {
      secure: true
    }
  })

  onMount(() => {
    const dropzone = new Dropzone('#dropzone', {
      uploadMultiple: false,
      acceptedFiles: '.jpg,.png,.webp',
      maxFiles: 1
    })

    dropzone.on('sending', (file, xhr, formData) => {
      imageStatus.set(ImageStatus.UPLOADING)
      formData.append('upload_preset', 'ml_default')
      formData.append('timestamp', Date.now() / 1000)
      formData.append('api_key', 114866565814265)
    })

    dropzone.on('success', (file, response) => {
      const { public_id: publicId, secure_url: url } = response

      const imageWhitoutBackground = cloudinary
        .image(publicId)
        .effect(backgroundRemoval())

      imageStatus.set(ImageStatus.DONE)
      originalImage.set(url)
      modifiedImage.set(imageWhitoutBackground.toURL())
    })

    dropzone.on('error', (file, response) => {
      console.log('its bad')
      console.log(response)
    })
  })
</script>

<form
  id="dropzone"
  class="dropzone shadow-2xl"
  action="https://api.cloudinary.com/v1_1/dkk9jnnus/image/upload"
>
  {#if $imageStatus === ImageStatus.READY}
    <button
      class="px-8 py-2 bg-[#64ffe360] text-white font-normal rounded-full pointer-events-none flex items-center justify-center gap-4 xl:px-16 xl:py-4 xl:text-2xl xl:gap-6"
      >Upload file <UploadIcon /></button
    >
    <span class="text-base text-neutral-500 text-center font-medium xl:text-3xl"
      >Or drop file</span
    >
    <ul class="flex items-center justify-center gap-2 xl:gap-4">
      <li class="text-xs text-neutral-400 text-center font-medium xl:text-lg">
        .jpg
      </li>
      <li class="text-xs text-neutral-400 text-center font-medium xl:text-lg">
        .png
      </li>
      <li class="text-xs text-neutral-400 text-center font-medium xl:text-lg">
        .webp
      </li>
    </ul>
  {/if}
  {#if $imageStatus === ImageStatus.UPLOADING}
    <Loader />
  {/if}
</form>

<style>
  #dropzone {
    padding: 40px 24px;
    width: 100%;
    min-height: auto;
    aspect-ratio: 16/9;
    color: #ffffff;
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    gap: 24px;
    background-color: #202124;
    border: 2px solid #38383e;
    border-radius: 16px;
    position: relative;
    z-index: 40;
  }

  :global(.dropzone.dz-clickable .dz-message) {
    display: none !important;
  }

  @media only screen and (min-width: 1280px) {
    #dropzone {
      gap: 32px;
      border-radius: 24px;
    }
  }
</style>
