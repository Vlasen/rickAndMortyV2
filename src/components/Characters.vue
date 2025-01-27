<script setup>
defineProps(["characters"])
const emit = defineEmits(["mouseMoveGradient", "mouseLeave"])

const mouseMoveGradient = (event) => {
  emit("mouseMoveGradient", event);
};
const mouseLeave = (event) => {
  emit("mouseLeave", event);
};

</script>

<template>
  <div class="characterBox">
    <div class="charactersList">
      <div class="characterCard"
        v-for="character in characters" :key="character.id">

        <div class="mainCharacterInfo">
          <img class="characterImage" :alt="character.name + ' image'" :src="character.image" />
          
          <div class="info">
            <div class="topGradientLayer" @mousemove="mouseMoveGradient" @mouseleave="mouseLeave"></div>
            <div class="nameCharacter"> {{ character.name }} </div>

            <div class="status">
              <svg
                xmlns="http://www.w3.org/2000/svg"
                width="20"
                height="20"
                viewBox="0 0 32 32"
              >
                <circle
                  v-if="character.status == 'Alive'"
                  cx="16"
                  cy="16"
                  r="8"
                  fill="#009e03"
                />
                <circle
                  v-if="character.status == 'Dead'"
                  cx="16"
                  cy="16"
                  r="8"
                  fill="#990000"
                />
                <circle
                  v-if="character.status == 'unknown'"
                  cx="16"
                  cy="16"
                  r="8"
                  fill="#bfbfbf"
                />
              </svg>
              <label>{{ character.status }}</label>
            </div>
            <div class="species"> {{ character.species }} </div>
          </div>
        </div>
      </div>
      
    </div>
    <div class="filterContainer">
      <label class="filterSection">Filters</label>
      <div class="nameBox">
        <label class="filterName">Name</label>
        <input type="text" class="inputFilterName"/>
      </div>
      <div class="statusBox">
        <label class="filterStatus">Status</label>
        <input type="text"/>
      </div>

    </div>
  </div>
</template>

<style lang="scss">
.characterBox {
  position: relative;
  top: max(65px, 10vh);
  right: 50px;
  margin-left: 99px;
  display: flex;
  flex-direction: row;
  justify-content: space-between;
  min-width: 640px;
  padding: 15px 15px 50px 15px;

  border-radius: 15px 15px 0 0;
  background: rgb(58, 58, 58);
  box-shadow: inset 0 0 4px rgb(0, 255, 119);
  
  .charactersList {  
    display: flex;
    flex-direction: row;
    flex-wrap: wrap;
    justify-content: space-around;
    align-items: center;
    box-shadow: none;
    width: 80%;

    .characterCard {
      width: 310px;
      height: 120px;
      margin: 13px 15px 12px 15px;
      border: solid 1px black;
      border-radius: 10px;
      background: rgb(48, 48, 48);
      .topGradientLayer {
        position: absolute;
        margin: -5px 0 0 -128px;
        width: 308px;
        height: 120px;
        border-radius: 10px;
        z-index: 1;
      }
      .characterImage {
        height: 120px;
        border-radius: 10px 0 0 10px;
        z-index: 2;
      }
      .mainCharacterInfo {
        display: flex;
        flex-direction: row;
        .info {
          display: flex;
          flex-direction: column;
          margin: 5px 0 0 10px;
          font-size: .8rem;
          
          .nameCharacter {
            font-family: "Jersey 15", serif;
            font-size: 1.8em;
            letter-spacing: 1px;
            text-shadow: 2px 2px 2px rgb(64, 181, 203);
            color: rgb(0, 255, 119);
          }
          .status {
            display: flex;
            flex-direction: row;
            align-items: center;
          }
        }
      }
    }
    .characterCard:hover {
      background: rgb(55, 55, 55);;
    }
    
  }
  .filterContainer {
    display: flex;
    flex-direction: column;
    align-items: center;
    min-width: 250px;
    width: 20%;
    border: solid red;

    .filterSection {
      margin: 30px 0 30px 0;
      font-family: "Jersey 15", serif;
      font-size: 3em;
      letter-spacing: 2px;
      text-shadow: 2px 2px 2px rgb(64, 181, 203);
      color: rgb(0, 255, 119);
    }
    .nameBox, .statusBox {
      display: flex;
      flex-direction: column;
      width: 80%;
      margin-bottom: 30px;

      .filterName, .filterStatus {
        font-size: 1.6em;
        margin: 0 0 10px 12px;
      }
      .inputFilterName {
        height: 1.2em;
        border-radius: 30px;
        padding: 7px 15px 7px 15px;
        font-family: "Unbounded", serif;
        color: black;
      } 
      .inputFilterName:focus {
        background: rgb(0, 255, 119);
        box-shadow: 0 0 10px rgb(64, 181, 203);
        outline:none;
      }
    }
  }
}
     
</style>