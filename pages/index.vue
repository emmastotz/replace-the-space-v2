<template>
  <div class="bg-gradient-to-tl from-blue-900 via-blue-700 to-blue-400 h-screen overflow-hidden">
    <div class="bg-gradient-to-tr from-red-600 to-red-900 h-1/4 flex items-center justify-center">
      <NuxtLink class="uppercase bg-red-100 text-red-600 font-semibold tracking-widest rounded-md px-4 shadow hover:shadow-lg hover:bg-white" to="/play">
        Play
      </NuxtLink>
    </div>
    <div class="flex px-12 justify-center">
      <img
        class="object-cover lg:max-w-8xl"
        src="@/assets/images/replacethespace.png"
        alt="Replace the Space Logo"
      >
    </div>
  </div>
</template>
