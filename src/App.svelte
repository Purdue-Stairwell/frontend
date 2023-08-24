<script>
  import Header from './lib/Header.svelte'
  import StaticImage from './lib/vbox/StaticImage.svelte';
  import Animation from './lib/vbox/Animation.svelte';
  import Squiggle from './lib/vbox/Squiggle.svelte';
  import Narration from './lib/ibox/Narration.svelte';
  import CreditsButton from './lib/abox/CreditsButton.svelte';
  import NextButton from './lib/abox/NextButton.svelte';
  import RefreshButton from './lib/abox/RefreshButton.svelte';

  let screenLocation = 0;

  function nextPage() {
    if(screenLocation < 18)
    screenLocation++;
  }
  
</script>

<main>
 <!--HEADER-->
 <Header></Header>
 
 <!--INFO BOX-->
 {#if screenLocation != 2}
 <Narration scriptPage={screenLocation}></Narration>
 {/if}
 <!--VISUAL BOX-->
 <Animation></Animation>
 <!--ACTION BOX-->
 {#if screenLocation == 18}
  <CreditsButton></CreditsButton>
 {:else}
 <NextButton on:message={nextPage}></NextButton>
 {/if}
</main>

<style>
  
  main {
    background-color: aliceblue;
    width: 100vw;
    height: 100vh;
  }
</style>
