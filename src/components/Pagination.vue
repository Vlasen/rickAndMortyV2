<script setup>
const props = defineProps(["currentPage", "pages", "previousPage", "nextPage"]);
const emit = defineEmits(["getCharacters"]);

function getCharacters(url) {
  emit("getCharacters", url);
}
</script>

<template>
  <footer>
    <div class="shadowBox">
      <button class="pageButton" id="previousPage"
        @click="getCharacters(previousPage)" 
        :disabled="!previousPage" 
      >
        previous
      </button>

      <div class="pageCurrent" id="currentPage"
      >
        {{ currentPage }}
      </div> 

      <button class="pageButton" id="nextPage" 
        @click="getCharacters(nextPage)"
        :disabled="!nextPage"
      >
        next
      </button>

      <div id="dots" v-if="currentPage != pages">...</div>

      <button class="pageButton" id="pages"
        v-if="currentPage != pages"
        @click="getCharacters('https://rickandmortyapi.com/api/character/?page=42')"
      >
        {{ pages }}
      </button>
    </div>
  </footer>
</template>

<style lang="scss">
.footer {
  position: fixed;
  bottom: -2px;
  left: calc(50% - 200px);
}
footer {
  @extend .footer;
  height: 52px;
  width: 402px;
  background: black;
  clip-path: polygon(0 50px, 0px 21px, 19px 1px, 381px 1px, 401px 21px, 401px 50px);
  z-index: 3;

  .shadowBox {
    @extend .footer;
    display: flex;
    flex-direction: row;
    align-items: center;
    justify-content: center;
    height: 50px;
    width: 400px;

    background: rgb(58, 58, 58);
    clip-path: polygon(1px 50px, 1px 20px, 20px 0, 380px 0, 400px 20px, 400px 50px);
    box-shadow: inset 0 0 4px rgb(0, 255, 119);
    
    .pageButton, .pageCurrent, #dots {
      display: inline-block;
      margin: 10px;
      background: rgb(58, 58, 58);
      border: none;

      font-family: "Jersey 15", serif;
      font-size: 1.7rem;
      letter-spacing: 2px;
      text-shadow: 2px 2px 2px rgb(64, 181, 203);
      color: rgb(0, 255, 119);
      z-index: 4;
    }
    .pageButton:hover {
      color: rgb(0, 205, 96);
    }
    .pageButton:disabled {
      color: rgb(0, 0, 0);
    }
  }
}
</style>