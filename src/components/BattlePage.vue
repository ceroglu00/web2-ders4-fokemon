<template>
  <div style="background-color: purple" class="text-center">
    <h1 style="color: white; text-transform: uppercase" class="py-4">
      Fokemon
    </h1>
  </div>

  <div class="mt-16 mx-10">
    <div id="Oyuncu">
      <h2 class="text-center">Oyuncu</h2>
      <v-progress-linear v-model="oyuncu.can" color="blue" height="40">
        <span style="font-size: 25px; font-weight: bold; color: black">
          {{ oyuncu.can }}
        </span>
      </v-progress-linear>
    </div>

    <div id="Canavar" class="mt-6">
      <h2 class="text-center">Canavar</h2>
      <v-progress-linear v-model="canavar.can" color="blue" height="40">
        <span style="font-size: 25px; font-weight: bold; color: black">
          {{ canavar.can }}
        </span>
      </v-progress-linear>
    </div>

    <section v-if="!oyunBittimi" class="mt-16">
      <v-btn block variant="tonal" class="mt-1" @click="AtakYap"
      >Atak Yap
      </v-btn
      >
      <v-btn block variant="tonal" class="mt-1"
             @click="PotBas"
      >Pot Bas
      </v-btn>
      <v-btn block variant="tonal" class="mt-1"
             @click="OzelAtakYap"
             :disabled="turSayisi % 3 == 0 ? false : true"
      >Özel Atak Yap
      </v-btn>
    </section>
    <section v-else class="text-center mt-16">
      <h2 v-if="oyuncu.can<=0">GAME OVER!</h2>
      <h2 v-else>BRAVO KAZANDINIZ!</h2>
      <v-btn @click="OyunuBaslat" color="purple" variant="tonal">Yeniden Başlayalımmı ?</v-btn>
    </section>

    <section class="mt-16">
      <h2 class="text-center">Savaş Kayıtları</h2>

      <p v-for="savaşKaydı in savaşKayıtları">
        <span v-if="savaşKaydı.kim=='Oyuncu'">
          Oyuncu Canavara {{savaşKaydı.vuruş}} vurdu
        </span>
        <span v-else>Canavar oyuncuya vurdu</span>
      </p>

    </section>

  </div>
</template>

<script setup>
import {ref} from "vue";

var canBarYüksekliği = 100;

var turSayisi = 0;
var oyunBittimi = false;

var savaşKayıtları = ref([]);

var oyuncu = ref({
  can: 100,
  saldırıGücü: 100,
});

var canavar = ref({
  can: 100,
  saldırıGücü: 100,
});

function AtakYap(min, max) {
  if (min == undefined || max == undefined) {
    min = 5;
    max = 10;
  }
  var randomSayı = RandomNumber(min, max);
  canavar.value.can = canavar.value.can - randomSayı;
  CanavarAtak();
  turSayisi++;
  SavaşKaydı("Oyuncu", "Canavar", randomSayı);


  if (oyuncu.value.can <= 0) {
    // CANAVAR KAZANDI
    // GAME OVER
    oyuncu.value.can = 0;
    oyunBittimi = true;
  }

  if (canavar.value.can <= 0) {
    // OYUNCU KAZANDI
    canavar.value.can = 0;
    oyunBittimi = true;
  }


}

function CanavarAtak() {
  var randomSayı = RandomNumber(6, 10);
  oyuncu.value.can = oyuncu.value.can - randomSayı;
  SavaşKaydı("Canavar", "Oyuncu", randomSayı);
}

function PotBas() {
  var randomSayı = RandomNumber(5, 10);
  oyuncu.value.can = oyuncu.value.can + randomSayı;
  SavaşKaydı("Oyuncu", "Oyuncu", -randomSayı);
  CanavarAtak();
  turSayisi++;
}

function OzelAtakYap() {
  // if(turSayisi % 3 == 0){
  AtakYap(10, 20);
  // }
}


function OyunuBaslat() {
  oyuncu.value.can = 100;
  canavar.value.can = 100;
  turSayisi = 0;
  oyunBittimi = false;
}

function SavaşKaydı(kim, kime, vuruş) {

  var SavaşKaydıObjesi = {
    kim: kim,
    kime: kime,
    vuruş: vuruş
  }

  savaşKayıtları.value.push(SavaşKaydıObjesi);
}


function RandomNumber(min, max) {
  return Math.floor(Math.random() * (max - min) + min);

  // var random = (Math.random()* (max-min)) + min;
  // random = Math.floor(random);
  // return random;
}
</script>

<style lang="scss" scoped></style>
