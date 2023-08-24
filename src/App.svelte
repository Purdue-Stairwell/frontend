<script>
  import Header from './lib/Header.svelte'
  import StaticImage from './lib/vbox/StaticImage.svelte';
  import Animation from './lib/vbox/Animation.svelte';
  import Squiggle from './lib/vbox/Squiggle.svelte';
  import Narration from './lib/ibox/Narration.svelte';
  import CreditsButton from './lib/abox/CreditsButton.svelte';
  import NextButton from './lib/abox/NextButton.svelte';
  import RefreshButton from './lib/abox/RefreshButton.svelte';
</script>

<main>
 <!--HEADER-->
 <Header></Header>
 <!--INFO BOX-->
 <Narration></Narration>
 <!--VISUAL BOX-->
 <Animation></Animation>
 <!--ACTION BOX-->
 <NextButton></NextButton>
</main>

<style>
  
  main {
    background-color: aliceblue;
    width: 100vw;
    height: 100vh;
  }
</style>
