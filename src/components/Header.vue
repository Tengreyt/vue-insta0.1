<template>
  <header class="absolute r-0 inset-x-0 top-0 z-50">
    <nav class="flex items-center justify-between p-6 lg:px-8" aria-label="Global">
      <div class="flex lg:flex-1">
        <a href="#" class="-m-1.5 p-1.5">
          <span class="sr-only">Your Company</span>
          <img class="h-8 w-auto" src="https://tailwindui.com/img/logos/mark.svg?color=indigo&shade=600" alt="" />
        </a>
      </div>
      <div class="flex lg:hidden">
        <button type="button" class="-m-2.5 inline-flex items-center justify-center rounded-md p-2.5 text-gray-700" @click="mobileMenuOpen = true">
          <span class="sr-only">Open main menu</span>
          <Bars3Icon class="h-10 w-10" aria-hidden="true" />
        </button>
      </div>
      <div class="hidden right-0 lg:flex lg:gap-x-12">
        <a v-for="item in navigation" :key="item.name" :href="item.href" class="flex justify-end text-sm font-semibold leading-6 text-gray-900">{{ item.name }}</a>
      </div>
      <div class="hidden lg:flex lg:flex-1 lg:justify-end">
        <a href="#" class="text-sm font-semibold leading-6 text-gray-900" @click.prevent="openLoginModal">
          Log in 
          <span aria-hidden="true">&rarr;</span>
        </a>
      </div>
    </nav>
    <Dialog class="lg:hidden" @close="mobileMenuOpen = false" :open="mobileMenuOpen">
      <div class="fixed inset-0 z-50" />
      <DialogPanel class="fixed inset-y-0 right-0 z-50 w-full overflow-y-auto bg-white px-6 py-6 sm:max-w-sm sm:ring-1 sm:ring-gray-900/10">
        <div class="flex items-center justify-between">
          <a href="#" class="-m-1.5 p-1.5">
            <span class="sr-only">Your Company</span>
            <img class="h-8 w-auto" src="https://tailwindui.com/img/logos/mark.svg?color=indigo&shade=600" alt="" />
          </a>
          <button type="button" class="-m-2.5 rounded-md p-2.5 text-gray-700" @click="mobileMenuOpen = false">
            <span class="sr-only">Close menu</span>
            <XMarkIcon class="h-10 w-10" aria-hidden="true" />
          </button>
        </div>
        <div class="mt-6 flow-root">
          <div class="-my-6 divide-y divide-gray-500/10">
            <div class="space-y-2 py-6">
              <a v-for="item in navigation" :key="item.name" :href="item.href" class="-mx-3 block rounded-lg px-3 py-2 text-base font-semibold leading-7 text-gray-900 hover:bg-gray-50">{{ item.name }}</a>
            </div>
            <div class="py-6">
              <a href="#" class="-mx-3 block rounded-lg px-3 py-2.5 text-base font-semibold leading-7 text-gray-900 hover:bg-gray-50">Log in</a>
            </div>
          </div>
        </div>
      </DialogPanel>
    </Dialog>

    <!-- Модальное окно для десктопной версии -->
    <div v-if="isLoginModalOpen" class="fixed inset-0 flex items-center justify-center bg-black bg-opacity-50 z-50">
      <div class="bg-white rounded-lg p-8 max-w-lg w-full">
        <h2 class="text-xl font-semibold mb-4">Log in</h2>
        <form @submit.prevent="submitLogin">
          <div class="mb-4">
            <label for="email" class="block text-sm font-medium text-gray-700">Email address</label>
            <input id="email" type="email" class="mt-1 block w-full px-3 py-2 border border-gray-300 rounded-md shadow-sm focus:ring-indigo-500 focus:border-indigo-500 sm:text-sm" v-model="email" required>
          </div>
          <div class="mb-4">
            <label for="password" class="block text-sm font-medium text-gray-700">Password</label>
            <input 
              @input="onChangeSearchInput"
              id="password" 
              type="password" 
              class="mt-1 block w-full px-3 py-2 border border-gray-300 rounded-md shadow-sm focus:ring-indigo-500 focus:border-indigo-500 sm:text-sm"  
              v-model="password" required>
          </div>
          <div class="flex justify-end">
            <button type="button" class="px-4 py-2 bg-gray-500 text-white rounded-lg mr-4" @click="closeLoginModal">Cancel</button>
            <button type="submit" class="px-4 py-2 bg-blue-500 text-white rounded-lg">Log in</button>
          </div>
        </form>
      </div>
    </div>
  </header>
</template>

<script setup>
import debounce from 'lodash/debounce';
import { reactive, ref, watch } from 'vue';
import axios from 'axios';
import { Bars3Icon, XMarkIcon } from '@heroicons/vue/24/outline';

const navigation = [
  { name: 'Product', href: '#' },
  { name: 'Features', href: '#' },
  { name: 'Marketplace', href: '#' },
  { name: 'Company', href: '#' },
];

const mobileMenuOpen = ref(false);
const isLoginModalOpen = ref(false);
const email = ref('');
const password = ref('');

const openLoginModal = () => {
  isLoginModalOpen.value = true;
};

const closeLoginModal = () => {
  isLoginModalOpen.value = false;
};

const filters = reactive({
  searchQuery: ''
})

const onChangeSearchInput = debounce((event) => {
  filters.searchQuery = event.target.value;
  console.log('Debounced input:', filters.searchQuery);
}, 500);

const submitLogin = async () => {
  try {
    const response = await axios.post('https://7jdnlbht-3000.euw.devtunnels.ms/', { // Измените на /author, если сервер ожидает этот путь
      email: email.value,
      password: password.value
    }, {
      headers: {
        'Accept': 'application/json',
        'Content-Type': 'application/json'
      }
    });

    console.log('Успешный вход:', response.data);
    closeLoginModal();
  } catch (error) {
    console.error('Ошибка при входе:', error);

    if (error.response) {
      console.error('Ответ сервера с ошибкой:', error.response.data);
    } else if (error.request) {
      console.error('Запрос был сделан, но ответа не получено:', error.request);
    } else {
      console.error('Произошла ошибка при настройке запроса:', error.message);
    }
  }
};

// Предположительно, вы хотите отслеживать изменения в `filters` через `watch`.
watch(filters);


</script>
