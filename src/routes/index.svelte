

<script>
import Kard from '$lib/Kard.svelte';

	import { range } from 'lodash-es';

	let disabled = false;
	let obtiznostString = '4';
    let obtiznost;
    let dvojice;
	const potvrd = () => {obtiznost = parseInt(obtiznostString)};
	function confirm() {
        potvrd();
		disabled = true;
        dvojice = (obtiznost**2)/2;
	}
	let karty = [];

    let posledniKrtek;
    $: console.log(dvojice);

    let cklickedcounter = 0;
    function klikaj(event){
        if(cklickedcounter < 2){      
            //   console.log(event.detail.piko());
            if (posledniKrtek?.cislo && posledniKrtek.cislo == event.detail.cislo) {
                event.detail.piko();
                posledniKrtek.piko();
                dvojice--;
            }
            posledniKrtek = undefined;
        }


       if(cklickedcounter > 1 ){
           cklickedcounter = 0;
           posledniKrtek = undefined;
       }
       else{
           cklickedcounter += 1;
           posledniKrtek = event.detail;
       }
    }

    // $: console.log(cklickedcounter);

function shuffle(array) {
  let currentIndex = array.length,  randomIndex;

  // While there remain elements to shuffle...
  while (currentIndex != 0) {

    // Pick a remaining element...
    randomIndex = Math.floor(Math.random() * currentIndex);
    currentIndex--;

    // And swap it with the current element.
    [array[currentIndex], array[randomIndex]] = [
      array[randomIndex], array[currentIndex]];
  }

  return array;
}

    $: {
        
        if(dvojice == 0){
            alert("Vyhral jsi, gratuluji");
        }
    }
	$: {

		karty = [];
		range(obtiznost ** 2).forEach((i) => karty.push(Math.floor(i/2) +1));
		karty = karty;
        shuffle(karty);
	}

	// $: console.log(obtiznostString);
</script>
<h1 class="text-center text-4xl bg-red-400 rounded max-w-min whitespace-nowrap mx-auto">Zvol si obtiznost Pechesa</h1>
<div class="flex flex-col items-center text-2xl">
	<div class="bg-blue-500 rounded">Obtížnost:
	<select class="bg-blue-300 rounded"name="" id="" {disabled} bind:value={obtiznostString}>
		<option value="4">Ejzy</option>
		<option value="6">Nourmal</option>
		<option value="8">Chard</option>
	</select>
    </div>
	<button class="bg-purple-300 rounded" on:click={confirm}>Potvrdit</button>
</div>
<div class="grid gap-4" style="grid-template-columns: repeat({obtiznost}, minmax(0, 1fr));">
	{#each karty as karta, i}
		<Kard on:kam={klikaj} {cklickedcounter} cislo={karta} />
	{/each}
</div>
