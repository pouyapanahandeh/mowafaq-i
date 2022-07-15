<template>
  <q-layout view="lHr lpR lfr">
    <q-header bordered class="bg-blue-grey-4 text-white">
      <q-toolbar>
        <q-toolbar-title id="headerTag">
          <!-- <q-avatar>
            <img src="https://cdn.quasar.dev/logo-v2/svg/logo-mono-white.svg" />
          </q-avatar> -->
          <span class="text-h5">Mowafaq Ismaeil</span>
        </q-toolbar-title>
        <q-btn flat round dense>
          <q-btn
            push
            bordered
            color="white"
            text-color="black"
            icon-right="fa-solid fa-arrow-right-long"
            label="request program"
          />
        </q-btn>
      </q-toolbar>
    </q-header>

    <q-page-container>
      <router-view />
    </q-page-container>

    <q-footer bordered class="bg-grey-8 text-white">
      <q-toolbar>
        <q-toolbar-title>
          <q-avatar>
            <img src="https://cdn.quasar.dev/logo-v2/svg/logo-mono-white.svg" />
          </q-avatar>
          <div>Title</div>
        </q-toolbar-title>
      </q-toolbar>
    </q-footer>
  </q-layout>
</template>
<style>
@import url("https://fonts.googleapis.com/css2?family=Chakra+Petch&display=swap");

#headerTag {
  font-family: "Chakra Petch", sans-serif;
}
</style>
