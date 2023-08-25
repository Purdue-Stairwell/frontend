<script>
  import Header from './lib/Header.svelte'
  import StaticImage from './lib/vbox/StaticImage.svelte';
  import Animation from './lib/vbox/Animation.svelte';
  import Squiggle from './lib/vbox/Squiggle.svelte';
  import Narration from './lib/ibox/Narration.svelte';
  import CreditsButton from './lib/abox/CreditsButton.svelte';
  import NextButton from './lib/abox/NextButton.svelte';
  import WhoFive from './lib/vbox/WhoFive.svelte';
  import ProjectButton from './lib/abox/ProjectButton.svelte';
  import Resources from './lib/Resources.svelte';

  import { onMount } from 'svelte';
	

  const script = fetch('script.json').then(res => res.json());

  let screenLocation = 0;

  let credits = false;

  let whoFiveScores = [5,5,5,5,5];

  function nextPage() {
    if(screenLocation < 18)
    screenLocation++;
  }

  function updateWhoFiveScore(event) {
    whoFiveScores[event.detail.number] = event.detail.answer;
  }

  function projectSquiggle() {
    alert("You have been squiggled!");
  }

  let canvasElement;
  onMount(() => {
        document.body.addEventListener(
          "touchstart",
          (e) => {
            if (e.target == canvasElement) {
              e.preventDefault();
            }
          },
          { passive: false }
        );
        document.body.addEventListener(
          "touchend",
          (e) => {
            if (e.target == canvasElement) {
              e.preventDefault();
            }
          },
          { passive: false }
        );
        document.body.addEventListener(
          "touchmove",
          (e) => {
            if (e.target == canvasElement) {
              e.preventDefault();
            }
          },
          { passive: false }
        );
  });

    
</script>

<main>
  <!--HEADER-->
  <Header></Header>
  {#if !credits}
    {#key screenLocation}
    <!--INFO BOX-->
      {#await script}
        <p>Loading Script :)</p>
      {:then result}
        <Narration scriptPage={result.script[screenLocation]}></Narration>
      {/await}
      {/key}
    <!--VISUAL BOX-->
    <!-- 0       1     2     3     4    5    6    7   8   9   10  11  12   13    14   15     16-->
    <!--static static squig animm anim anim anim who who who who who anim anim squig static static-->
    {#if screenLocation < 2 || screenLocation > 14}
      <StaticImage screenLocation={screenLocation}></StaticImage>
    {:else if screenLocation >= 7 && screenLocation <= 11}
      <WhoFive on:whofive={updateWhoFiveScore} questionNumber={screenLocation-7} value={whoFiveScores[screenLocation-7]}></WhoFive>
    {:else if screenLocation == 2 || screenLocation == 14}
      <Squiggle bind:this={canvasElement} sketchWidth={300} sketchHeight={300}></Squiggle>
    {:else}
      <Animation></Animation>
    {/if}
    <!--ACTION BOX-->
    {#if screenLocation == 18}
      <CreditsButton on:credits={() => (credits = true)}></CreditsButton>
    {:else if screenLocation == 14}
      <ProjectButton on:message={nextPage} on:project={projectSquiggle}></ProjectButton>
    {:else}
      <NextButton on:message={nextPage}></NextButton>
    {/if}
  {:else}
    <Resources></Resources>
  {/if}
  <div id="purduep">
    <img src="./justthep.png" alt="Purdue P Logo">
  </div>
</main>

<style>

  #purduep {
    margin-top: 5px;
  }
  
  main {
    background-color: #FFFFFF;
    margin: 0 auto;
    width: 390px;
    height: 844px;
  }
  img {
    width: 50%;
    margin: 0 auto;
  }

  div {
    width: 100%;
    display: flex;
    justify-content: center;
  }
</style>
