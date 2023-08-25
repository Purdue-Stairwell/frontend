<script>
  import Header from './lib/Header.svelte'
  import StaticImage from './lib/vbox/StaticImage.svelte';
  import Animation from './lib/vbox/Animation.svelte';
  import Squiggle from './lib/vbox/Squiggle.svelte';
  import Narration from './lib/ibox/Narration.svelte';
  import CreditsButton from './lib/abox/CreditsButton.svelte';
  import NextButton from './lib/abox/NextButton.svelte';
  import RefreshButton from './lib/abox/RefreshButton.svelte';
  import WhoFive from './lib/vbox/WhoFive.svelte';
  import ProjectButton from './lib/abox/ProjectButton.svelte';

  import { crossfade } from 'svelte/transition';
  import { quintOut } from 'svelte/easing';
  import { onMount } from 'svelte';

	

  const [send, receive] = crossfade({
    duration: 1500,
    easing: quintOut
  });

  const script = fetch('script.json').then(res => res.json());

  let screenLocation = 0;

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
        console.log(canvasElement)
        document.body.addEventListener(
          "touchstart",
          (e) => {
            console.log(e.target);
            if (e.target == canvasElement) {
              console.log("bingo");
              e.preventDefault();
            }
          },
          { passive: false }
        );
        document.body.addEventListener(
          "touchend",
          (e) => {
            if (e.target == canvasElement) {
              console.log("bingo");
              e.preventDefault();
            }
          },
          { passive: false }
        );
        document.body.addEventListener(
          "touchmove",
          (e) => {
            if (e.target == canvasElement) {
              console.log("bingo");
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
  <CreditsButton></CreditsButton>
 {:else if screenLocation == 14}
 <ProjectButton on:message={nextPage} on:project={projectSquiggle}></ProjectButton>
 {:else}
 <NextButton on:message={nextPage}></NextButton>
 {/if}
 
</main>

<style>
  
  main {
    background-color: #246EB9;
    margin: 0 auto;
    width: 390px;
    height: 844px;
  }
</style>
