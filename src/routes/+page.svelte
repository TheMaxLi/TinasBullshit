<script lang="ts">
  import { backInOut, linear } from "svelte/easing";
  let springy = true;

  let flippedMax = $state(false);
  let runningAudioContext = $state<null | HTMLAudioElement>(null);
  let duration = $derived(springy ? 2000 : 1000);
  let easing = $derived(springy ? backInOut : linear);
  let options = $derived({ duration, easing, times: 2 });

  $effect(() => {
    console.log(flippedMax);
    runningAudioContext?.play();
    runningAudioContext!.currentTime = 0;
  });

  function spin(
    node: Element,
    options: { duration: number; easing: (t: number) => number; times: number }
  ) {
    const { times = 1 } = options;
    return {
      ...options,
      // The value of t passed to the css method
      // varies between zero and one during an "in" transition
      // and between one and zero during an "out" transition.
      css(t: number) {
        const degrees = 360 * times;
        return `transform: scale(${t}) rotate(${t * degrees}deg);`;
      },
    };
  }

  let videoElement = $state<null | HTMLVideoElement>(null);
  $effect(() => {
    if (navigator.mediaDevices.getUserMedia) {
      navigator.mediaDevices
        .getUserMedia({ video: true })
        .then(function (stream) {
          videoElement!.srcObject = stream;
        })
        .catch(function (err0r) {
          console.log("Something went wrong!");
        });
    }
  });
</script>

<div
  class="w-[16046px] relative flex bg-[url(/background.png)] overflow-y-hidden"
>
  <div class="w-[2721px] pl-32">
    <div
      class="w-[2721px] text-center justify-center text-white text-[450px] font-normal font-['Rungli']"
    >
      tina’s bullshit
    </div>
  </div>
  <img
    class="w-[1235px] h-screen pl-[300px]"
    src="/max-monkey.png"
    alt="max-monkey friend"
  />
  <div class="relative pl-[400px]">
    <!-- svelte-ignore a11y_no_static_element_interactions -->
    <div
      onmouseenter={() => (flippedMax = true)}
      onmouseleave={() => (flippedMax = false)}
      class="absolute w-[1235px]"
      style="transform: translate(0%, -50%);"
    >
      {#if flippedMax}
        <img
          transition:spin={options}
          class="w-[1235px] h-screen transform"
          src="/max-img.png"
          style="transform: translate(20%, 0%);"
          alt="husband to-be"
        />
      {:else}
        <img
          transition:spin={options}
          class="w-[1235px] h-screen transform rotate-180"
          src="/max-img.png"
          style="transform: translate(20%, -50%);"
          alt="husband to-be"
        />{/if}
    </div>
    <audio bind:this={runningAudioContext} controls class="hidden"
      ><source src="/running.mp3" /></audio
    >
  </div>
  <div class="h-screen pl-[1250px] flex justify-center items-center">
    <iframe
      title="spotify playlist justin bieber"
      style="border-radius:12px"
      src="https://open.spotify.com/embed/playlist/058JftPsFYN0ojTxrGtXyc?utm_source=generator"
      width="600"
      height="552"
      frameBorder="0"
      allowfullscreen={false}
      allow="autoplay; clipboard-write; encrypted-media; fullscreen; picture-in-picture"
      loading="lazy"
    ></iframe>
  </div>
  <div class="flex justify-center items-center pl-[400px]">
    <div class="justify-start w-[330px]">
      <span
        class="text-white text-2xl font-normal font-['Helvetica'] lowercase leading-10"
        >seafood boil with max and tina<br /><br />seasoned water<br /></span
      ><span
        class="text-white text-2xl font-normal font-['Helvetica'] lowercase leading-10"
        >• old bay<br />• paprika<br />• cajun<br />• lemon pepper<br />• garlic
        powder<br />cayenne pepper<br /></span
      ><span
        class="text-white text-2xl font-normal font-['Helvetica'] lowercase leading-10"
        ><br />sauce<br /></span
      ><span
        class="text-white text-2xl font-normal font-['Helvetica'] lowercase leading-10"
        >• butter<br />• minced garlic<br />• diced onions<br />• lemon<br
        /></span
      ><span
        class="text-white text-2xl font-normal font-['Helvetica'] lowercase leading-10"
        ><br />the other shit<br /></span
      ><span
        class="text-white text-2xl font-normal font-['Helvetica'] lowercase leading-10"
        >• crab<br />• shrimp<br />• corn<br />• sausages</span
      >
    </div>
  </div>
  <div class="w-[2721px]">
    <div
      class="w-[2721px] text-center justify-center text-white text-[450px] italic font-rungli-italic"
    >
      tina’s beef
    </div>
  </div>
  <img alt="wife to-be" class="w-auto h-screen" src="/tina-img.png" />
  <video autoplay={true} bind:this={videoElement} class="pl-[400px]"
    ><track kind="captions" />
  </video>
  <div class="h-screen items-center flex w-[1000px] pl-[300px]">
    <img alt="i hate next.js" class="rounded-[20px]" src="/error.png" />
  </div>
  <p class="absolute bottom-0 text-white right-0">Tina Duong</p>
</div>
