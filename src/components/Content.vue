<script setup>
import Characters from "@/components/Characters.vue"
import Locations from "@/components/Locations.vue"
import Pagination from "@/components/Pagination.vue"
import { onMounted, reactive } from "vue"
import axios from "axios"

const state = reactive({
  charactersResult: [],
  locationsResult: [],
  episodesResult: [],
  pages: 0,
  previousPage: "",
  currentPage: 1,
  nextPage: "",
  buttons: [{
    name: 'charactersButton',
    status: true,
  }, 
  {
    name: 'locationsButton',
    status: false,
  },
  {
    name: 'episodesButton',
    status: false,
  }],
  filterName: "",
  filterStatus: "",
});

function buttonActive(event) {
  let elem = event.target;
  for (let i=0; i<state.buttons.length; i++) {
    if (elem.getAttribute('id') == state.buttons[i].name) {
      state.buttons[i].status = true;
      elem.classList.add("active");
      elem.classList.remove("notactive");
    } else {
      state.buttons[i].status = false;
      let anotherButtons = document.getElementById(state.buttons[i].name);
      anotherButtons.classList.remove("active");
      anotherButtons.classList.add("notactive");
    }
  }
};

async function getCharacters(url) {
  try {
    const response = await axios.get(url);
    state.charactersResult = response.data.results;
    state.pages = response.data.info.pages;
    state.previousPage = response.data.info.prev;
    state.nextPage = response.data.info.next;

    if (state.previousPage) {
      let array = state.previousPage.split("page=");
      state.currentPage = Number(array[1]) + 1;
    } else if (state.nextPage) {
      let array = state.nextPage.split("page=");
      state.currentPage = Number(array[1]) - 1;
    }
  } catch(error) {
    console.error("Error getting", error);
  }
};

async function characterFilter(name, status) {
  try {
    const response = await axios.get(
      `https://rickandmortyapi.com/api/character/?name=${name}&status=${status}`
    );
    state.charactersResult = response.data.results;
    state.pages = response.data.info.pages;
    state.previousPage = response.data.info.prev;
    state.nextPage = response.data.info.next;

    if (state.previousPage) {
      let array = state.previousPage.split("page=");
      state.currentPage = Number(array[1]) + 1;
    } else if (state.nextPage) {
      let array = state.nextPage.split("page=");
      state.currentPage = Number(array[1]) - 1;
    }
  } catch(error) {
    console.error("Error getting", error);
  }
  state.filterName = "";
  state.filterStatus = "";
}

async function getLocations(url) {
  try {
    const response = await axios.get(url);
    state.locationsResult = response.data.results;
    state.pages = response.data.info.pages;
    state.previousPage = response.data.info.prev;
    state.nextPage = response.data.info.next;

    if (state.previousPage) {
      let array = state.previousPage.split("page=");
      state.currentPage = Number(array[1]) + 1;
    } else if (state.nextPage) {
      let array = state.nextPage.split("page=");
      state.currentPage = Number(array[1]) - 1;
    }
  } catch(error) {
    console.error("Error getting", error);
  }
};

function mouseMoveGradient(event) {
  let elem = event.target;
  if (elem.getAttribute('class') == "topGradientLayer") {
    let box = elem.getBoundingClientRect();
    let X = event.clientX - box.left;
    let Y = event.clientY - box.top;

    let xPercent = X / (box.width / 100);
    let yPercent = Y / (box.height / 100);
    elem.style.background = `radial-gradient(circle 35px at ${xPercent}% ${yPercent}%, rgb(0, 255, 119, 0.5), rgb(48, 48, 48, 0))`;
  }
};
function mouseLeave(event) {
  let elem = event.target;
  if (elem.getAttribute('class') == "topGradientLayer") {
    elem.style.background = "none";
  }
};

onMounted(() => {
  characterFilter(state.filterName, state.filterStatus);
  document.getElementById('charactersButton').classList.add("active");
  document.getElementById('locationsButton').classList.add("notactive");
  document.getElementById('episodesButton').classList.add("notactive");
});
</script>

<template>
  <main>
    <div class="buttonBox" @click="buttonActive">
      <div class="buttonContainer">
        <div class="button" id="charactersButton" 
        @click="characterFilter(state.filterName, state.filterStatus)">characters</div>
        <div class="shadowBox" id="charactersShadow"></div>
      </div>
      <div class="buttonContainer">
        <div class="button" id="locationsButton" 
            @click="getLocations">locations</div>
        <div class="shadowBox" id="locationsShadow"></div>
      </div>
      <div class="buttonContainer">
        <div class="button" id="episodesButton">episodes</div>
        <div class="shadowBox" id="episodesShadow"></div>
      </div>
    </div>

      <Characters 
        :characters="state.charactersResult"
        @mouseLeave="mouseLeave"
        @mouseMoveGradient="mouseMoveGradient"
        v-if="state.buttons[0].status"
      />
      <Locations 
        :locations="state.locationsResult"
        @mouseLeave="mouseLeave"
        @mouseMoveGradient="mouseMoveGradient"
        v-if="state.buttons[1].status"
      />
      <Pagination 
        :currentPage="state.currentPage"
        :pages="state.pages"
        :nextPage="state.nextPage"
        :previousPage="state.previousPage"
        @getCharacters="getCharacters"
      />

  </main>
</template>

<style lang="scss">
main {
    .buttonBox {
      position: fixed;
      bottom: 16px;
      left: 18px;
      width: 40px;
      height: 565px;
      
      @media (max-height: 665px) {
        position: absolute;
        top: max(90px, 12vh);
        left: 18px;
      }
      .button {
        position: absolute;
        display: flex;
        justify-content: center;
        align-items: center;

        width: 200px;
        height: 40px;
        clip-path: polygon(0 0, 180px 0, 200px 20px, 200px 40px, 0 40px);
        transform: rotate(-90deg);
        box-shadow: inset 0 0 4px rgb(0, 255, 119);

        font-family: "Jersey 15", serif;
        font-size: 1.8em;
        letter-spacing: 2px;
      }

      .active {
        background: rgb(0, 255, 119);
        text-shadow: 2px 2px 2px rgb(64, 181, 203);
        color: black;
      }
      .active:hover { 
        background: rgb(68, 246, 151);
      }
      .active:active {
        background: rgb(0, 185, 86);
      }

      .notactive {
        background: rgb(58, 58, 58);
        text-shadow: 2px 2px 2px rgb(64, 181, 203);
        color: rgb(0, 255, 119);
      }
      .notactive:hover {
        background: rgb(67, 67, 67);
      }
      .notactive:active {
        background: rgb(46, 46, 46);
      }

      characters {
        bottom: 445px;
        left: -80px;
      }
      locations {
        bottom: 264px;
        left: -80px;
      }
      episodes {
        bottom: 83px;
        left: -80px;
      }
      #charactersButton {
        @extend characters;
        z-index: 2;
      }
      #locationsButton {
        @extend locations;
        z-index: 4;
      }
      #episodesButton {
        @extend episodes;
        z-index: 6;
      }

      .shadowBox {
        position: absolute;
        width: 202px;
        height: 40px;
        background: black;
        clip-path: polygon(1px 0, 182px 0, 202px 20px, 202px 41px, 1px 41px);
        transform: rotate(-90deg);
        
        margin-left: -1px;
      }
      #charactersShadow {
        @extend characters;
        z-index: 1;
      }
      #locationsShadow {
        @extend locations;
        z-index: 3;
      }
      #episodesShadow {
        @extend episodes;
        z-index: 5;
      }
    }
}
</style>