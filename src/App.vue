<script setup lang="ts">
import { ref } from 'vue'

const orderNumber = ref('')
const resultData = ref(null)

const useFetch = async () => {
  if (!orderNumber.value) {
    resultData.value = 'Por favor, ingresa un número de orden válido.'
    return
  }

  try {
    const res = await fetch(`http://127.0.0.1:8000/api/v1/orders/search?query=${orderNumber.value}`)
    if (!res.ok) throw new Error('No se encontró el pedido.')
    const data = await res.json()
    resultData.value = data.length ? data[0] : 'No se encontraron resultados.'
  } catch (err) {
    resultData.value = 'Error al buscar el pedido: ' + err.message
  }
}

const handleSubmit = (e: Event) => {
  e.preventDefault()
  useFetch()
}
</script>

<template>
  <main class="w-screen h-screen flex">
    <aside
      class="w-[400px] bg-white border-r border-gray-200 shadow-xl p-10 flex flex-col justify-center rounded-lg"
    >
      <h2 class="text-4xl font-semibold text-gray-800 mb-8 text-center">Buscar Pedido</h2>

      <form @submit="handleSubmit" class="flex flex-col gap-6">
        <div class="flex flex-col gap-1">
          <label for="order" class="text-gray-700 text-sm font-medium">Número de Pedido</label>
          <input
            id="order"
            v-model="orderNumber"
            type="text"
            placeholder="Ej. Orden #12345"
            class="h-[44px] px-4 rounded-lg border border-gray-300 bg-gray-50 focus:outline-none focus:ring-2 focus:ring-indigo-500 text-gray-800 placeholder-gray-400 transition"
          />
        </div>

        <button
          type="submit"
          class="mt-2 bg-indigo-600 text-white py-2.5 rounded-lg hover:bg-indigo-700 transition font-medium shadow-xl transform hover:scale-105"
        >
          Buscar
        </button>
      </form>
    </aside>

    <section class="flex-1 flex items-center justify-center p-10">
      <div v-if="resultData" class="w-full max-w-3xl rounded-2xl p-8 text-gray-700">
        <div v-if="typeof resultData === 'object'" class="border border-gray-200 shadow-lg">
          <h3 class="text-xl font-semibold text-gray-900 mb-4">Detalles del Pedido</h3>
          <div class="space-y-6">
            <div class="flex justify-between">
              <div id="result">
                <strong class="text-gray-700">Número de Pedido:</strong>
                <p class="text-lg text-indigo-600">{{ resultData.id }}</p>
              </div>
              <div class="flex items-center" id="result">
                <span
                  :class="{
                    'text-red-500': resultData.status === 'canceled',
                    'text-green-500': resultData.status === 'completed',
                    'text-yellow-500': resultData.status === 'pending',
                  }"
                >
                  <i
                    :class="{
                      'fas fa-times-circle': resultData.status === 'canceled',
                      'fas fa-check-circle': resultData.status === 'completed',
                      'fas fa-hourglass-half': resultData.status === 'pending',
                    }"
                  ></i>
                </span>
                <p class="ml-2 font-semibold">
                  {{ resultData.status.charAt(0).toUpperCase() + resultData.status.slice(1) }}
                </p>
              </div>
            </div>

            <div id="result">
              <strong class="text-gray-700">Cliente ID:</strong>
              <p>{{ resultData.customer_id }}</p>
            </div>
            <div id="result">
              <strong class="text-gray-700">Material ID:</strong>
              <p>{{ resultData.material_id }}</p>
            </div>
            <div id="result">
              <strong class="text-gray-700">Fecha de Pedido:</strong>
              <p>{{ resultData.date }}</p>
            </div>
          </div>
        </div>

        <div v-else>
          <p class="text-center text-gray-600">{{ resultData }}</p>
        </div>
      </div>
    </section>
  </main>
</template>

<style scoped>
h2,
h3 {
  font-family: 'Poppins', sans-serif;
}

input[type='text'] {
  font-family: 'Roboto', sans-serif;
  border: none;
}

button {
  transition: transform 0.3s ease;
}

button:hover {
  transform: scale(1.05);
}

button:active {
  transform: scale(1);
}

section div {
  padding: 1.5rem;
  background: rgba(255, 255, 255, 0.8);
  backdrop-filter: blur(10px);
  transition: transform 0.3s ease-in-out;
}

section div:hover {
  transform: translateY(-5px);
}

#result {
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
}

.fas {
  font-size: 1.2rem;
  transition: color 0.3s;
}
</style>
