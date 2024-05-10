<script setup lang="ts">
import { ref } from 'vue';
import axios from 'axios';

const url = ref()
const data = ref()
const onClick = async () =>{
  console.log("hello world")
  console.log(url.value)

      try {
        const response = await axios.get(url.value);
        data.value = response.data;
        console.log(url.value)
      } catch (error) {
        console.error(error);
      }
}

const fetchJokes =async ()=> {
  try {
    const response = await fetch('https://stackoverflow.com/questions/1533510/rest-standard-for-get-on-a-resource-that-doesnt-exist');
    const jokes = await response.json();
    console.log(jokes)
  } catch (error) {
    console.error(error);
  }
}
</script>


<template>
  <div class="px-6 pt-14 lg:px-8">
<div>
  <h1>Get html</h1>
  <div class="relative mt-2 rounded-md shadow-sm">
    <input 
    type="url" name="url" id="url" 
    v-model="url"
    class="block w-full rounded-md border-0 py-1.5 pl-3 pr-20 text-gray-900 ring-1 ring-inset ring-gray-300 placeholder:text-gray-400 focus:ring-2 focus:ring-inset focus:ring-indigo-600 sm:text-sm sm:leading-6"
     placeholder="https://hoge.hoge"
     >
   
  </div>
</div>
<div class="mt-3 flex items-center justify-end gap-x-6">
    <button @click="fetchJokes" class="rounded-md bg-indigo-600 px-3 py-2 text-sm font-semibold text-white shadow-sm hover:bg-indigo-500 focus-visible:outline focus-visible:outline-2 focus-visible:outline-offset-2 focus-visible:outline-indigo-600">Get</button>
  </div>
</div>

<iframe src="https://stackoverflow.com/questions/1763479/how-to-get-the-html-for-a-dom-element-in-javascript" width="100%" height="500px">
    <p>Your browser does not support iframes.</p>
</iframe>

</template>

