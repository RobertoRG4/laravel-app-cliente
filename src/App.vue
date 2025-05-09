<script setup lang="ts"></script>

<template>
  <main class="w-screen h-screen">
    <section class="flex justify-center items-center h-[200px] text-center">
      <form class="flex flex-col">
        <label class="text-2xl">Nombre del pedido</label>
        <input type="text" class="border rounded-xl w-[300px] h-[35px]" />
        <div class="pt-5">
          <button class="border rounded w-fit px-5">Buscar</button>
        </div>
      </form>
    </section>
    <section class="flex justify-center items-center">
      <div class="border rounded w-[550px] h-[500px]"></div>
    </section>
  </main>
</template>
