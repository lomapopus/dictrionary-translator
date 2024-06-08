<script setup lang="ts">



  import { invoke } from '@tauri-apps/api/core';
  import { onMount } from 'svelte';


  //let db;
  // onMount(async () => {
  //   db = await Database.load("sqlite:test.db");

  //   const result = await db.execute(
  //   "INSERT into words (en_word, ru_word) VALUES ($1, $2)",
  //   ["kaka", "popka"],
  //   );
  //   console.log("mount")
  // })

  //const db = await Database.load("sqlite:test.db");
  
  type translate_item = {
    ru: string,
    en: string,
    id: number
  }
  
  let item: translate_item = {
      en: "",
      ru: "",
      id: 0
  }
    let text_items = [item]
    async function translate_word(word: string, from: string = "en", to: string = "ru") {
      //const db = await Database.load("sqlite:test.db");  

      text_items.forEach(item => {
        console.log(item.en + ": " + item.ru)
      });
      if (text_items[text_items.length - 1].en != ""){  // добавление пустой записи
            text_items = [...text_items, {en: '', ru: '', id: item.id + 1}]
          }
  
      if (from == "en" && to == "ru") return invoke<string>("translate_from_en_to_ru", {word: word})
  
      if (from == "ru" && to == "en") return invoke<string>("translate_from_ru_to_en", {word: word})
  
      return ""
    }
  
  </script>

  {#each text_items as item}
    <form class="list_element">
      <div class="input">
        <input placeholder="Введи английское слово/фразу" bind:value={item.en} on:change={()=>translate_word(item.en, 'en', 'ru').then(res => item.ru = res)}/>
        <p>EN</p>
      </div>
      <div class="input">
        <input bind:value={item.ru} on:change={()=>translate_word(item.ru, 'ru', 'en').then(res => item.en = res)}/>
        <p>RU</p>
      </div>
    </form>
  {/each}

<style>
body { 
  margin: 0;   /* Remove body margins */
}

:root {
  color: #f6f6f6;
  background-color: #151515;
}

.container {
  margin: 0;
  padding-top: 5vh;
  display: flex;
  flex-direction: column;
  justify-content: center;
  text-align: center;
}

input,
button {
  outline: none;
  color: #c0c1c7;
  background-color: #0f0f0f98;
  border: 0px transparent;
  padding: 0.6em 0.8em;
  font-size: 1.15em;
  font-weight: 800;
  font-family: "Roboto Mono", monospace;
  font-optical-sizing: auto;
  width: 100%;
}

div.input {
  border-radius: 7px;
  background-color: #0f0f0f98;
  border: 2px solid #424244;
  display: flex;
  width: 100%;
  padding:  0 3px 0 0;
  flex-direction: row;
}

form.list_element {
  border-bottom: #313131 1px solid;
  border-top: #313131 1px solid;
  padding: 8px;
  gap: 4vw;
  display: flex;
  justify-content: center;
  flex-basis: 50%;
}

button {
  cursor: pointer;
}



p {
  background-color: #0f0f0f98;
  margin: 0;
  color: #c0c1c7;
  font-size: 0.8em;
  vertical-align: text-bottom;
  font-family: "Roboto Mono", monospace;
  font-optical-sizing: auto;
}

div.input:hover {
  border-radius: 7px;
  background-color: #0f0f0f98;
  border: 2px solid #ffffff;
  display: flex;
  width: 100%;
  padding:  0 3px 0 0;
  flex-direction: row;
}

button:hover {
  border-color: #396cd8;
}
button:active {
  border-color: #424244;
  background-color: #0f0f0f69;
}

</style>
