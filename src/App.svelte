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
  
</script>

<main>
 <!--HEADER-->
 <Header></Header>
 {#key screenLocation}
 <!--INFO BOX-->
 {#if screenLocation != 2 && screenLocation != 13}
  {#await script}
    <p>Loading Script :)</p>
  {:then result}
    <Narration scriptPage={result.script[screenLocation]}></Narration>
  {/await}
 {/if}
 <!--VISUAL BOX-->
 <!-- 0       1     2     3     4    5    6    7   8   9   10  11  12   13    14     14-->
 <!--static static squig animm anim anim anim who who who who who anim squig static static-->
 {#if screenLocation < 2 || screenLocation > 13}
 <StaticImage></StaticImage>
 {:else if screenLocation > 6 && screenLocation < 12}
 <WhoFive on:whofive={updateWhoFiveScore} questionNumber={screenLocation-7} value={whoFiveScores[screenLocation-7]}></WhoFive>
 {:else if screenLocation == 2 || screenLocation == 13}
 <Squiggle></Squiggle>
 {:else}
 <Animation></Animation>
 {/if}
 <!--ACTION BOX-->
 {#if screenLocation == 18}
  <CreditsButton></CreditsButton>
 {:else}
 <NextButton on:message={nextPage}></NextButton>
 {/if}
 {/key}
</main>

<style>
  
  main {
    background-color: aliceblue;
    width: 100vw;
    height: 100vh;
  }
</style>
