<template>
    <div>
        <p class="eyebrow"> <span>CLIMAEXPERT </span>Branding & Webdesign </p>
        <div style="display:flex">

            <h1 style="margin-bottom: 28px;">Offerte |</h1> <input type="text" placeholder="Projektname" style="font-weight: 900;"> 
        </div>
    </div>


<section class="body--container">
<section>

    <div class="main--grid">
  <div
    class="column--wrapper"
    v-for="(column, colIndex) in columns"
    :key="colIndex"
  >
    <div class="column--header">
      <p>{{ column.name }}</p>
    </div>

    <div
      class="column--element"
      v-for="(element, elIndex) in column.elements"
      :key="elIndex"
      :class="{ disabled: !element.enabled }"
    >
        <input
            class="element--name"
            v-model="element.name"
            @click.shift="toggleElement(column, element)"
            @click.alt.meta="duplicateElement(column, element)"
            @click.shift.meta="removeElement(column, element)"
            @keyup.enter="($event) => $event.target.blur()"

            :class="{
                'low-hours': element.hours <= 2,
                'medium-hours': element.hours > 2 && element.hours <= 5,
                'high-hours': element.hours > 5
            }"
            placeholder="Enter name"
        />
        
      <div class="element--hours">
        <p
        @click="addHours(element, $event)"


        >{{ getEffectiveHours(element) }}h</p>
      </div>
    </div>

    <!-- Arrow + column total at bottom -->
    <div
      style="width: calc(100% - 34px); display: flex; flex-direction: column; align-items: center; gap: 10px;"
    >
      <svg
        style="width: 20px;"
        xmlns="http://www.w3.org/2000/svg"
        viewBox="0 0 32 32"
      >
        <path
          d="M26.29 20.29 18 28.59V0h-2v28.59l-8.29-8.3-1.42 1.42 10 10a1 1 0 0 0 1.41 0l10-10z"
          data-name="2-Arrow Down"
        />
      </svg>
      <h1 class="columnSum">{{ getColumnTotal(column) }}h</h1>
    </div>
  </div>
</div>


    <div class="macro--controls">

        <button
        @click="toggleButtonSeitenzahl"
        :class="{inactive: buttonState !== 'single'}"
    >single site</button>
    
    <button
        @click="toggleButtonSeitenzahl" 
        :class="{inactive: buttonState !== 'multi'}"
    >multi-site</button>


</div>
<div class="preis--wrapper">
        <section class="preis--container">

            <div>
                <p>Stundenaufwand</p>
                <h1>{{ totalHours }}h</h1>
            </div>
            <div :style="{ opacity: columnSum > 5000 ? 0.2 : 1 }">
                <p >Preis</p>
                <h1>
                {{ columnSum }}.-</h1>
            </div>
            <div :style="{ opacity: columnSum < 5000 ? 0.2 : 1 }">
                <p>Preis mit Grössenrabatt*</p>

                <h1
                v-if="columnSum<5000"
                >ohne</h1>
                <h1
                v-else
                >{{ Math.floor((columnSum * 0.85)/10) *10}}.-</h1>
            </div>
            
        </section>
    </div>





 </section>
</section>

<p class="annotations">*Weil grössere Aufträge viel Zeit in Administration und Akquise sparen, können sie kostengünstiger angeboten werden.</p>


<p class="foothair"> {{today}} <span>&pfeifer </span> </p>

</template>

<script setup>

function addHours (element , event) {
    if (event.shiftKey) {

        element.hours-=1;
        if (element.hours<0) {
            element.hours=0;
        }
    }

    else {
        element.hours+=1;

    }
}




const buttonState = ref('multi') // or ref(true/false)

// Fix toggle function
function toggleButtonSeitenzahl() {
    if (buttonState.value === 'single') {
        buttonState.value = 'multi'
        seitenzahl.value = 1.5
        console.log('switched to multi-site')
    } else {
        buttonState.value = 'single' 
        seitenzahl.value = 1
        console.log('switched to single-site')
    }
}

const totalHours = computed(() => {
    return columns.value.reduce((sum, col) => sum + getColumnTotal(col), 0);
})

const getColumnTotal = (column) => {
    return column.elements.reduce((sum, el) => sum + getEffectiveHours(el), 0);
}

const getEffectiveHours = (element) => {
    return element.enabled ? element.hours * seitenzahl.value : 0;
}

function toggleElement(column, element) {
    element.enabled = !element.enabled;
    console.log('Element toggled:', element.name, 'Enabled:', element.enabled)
}

const columnSum = computed(() => totalHours.value * stundenLohn)


function removeElement(column, element) {
  const index = column.elements.indexOf(element)
  if (index !== -1) {
    column.elements.splice(index, 1)  // <-- reactive removal
  }
}


function duplicateElement(column, element) {
  const index = column.elements.indexOf(element)
  if (index !== -1) {
    // Create a shallow copy of the element
    const newElement = { ...element }
    // Insert it after the original
    column.elements.splice(index + 1, 0, newElement)
  }
}

const stundenLohn = 150;

const columns = ref([
    {
        name: "Research / Marketing",
        elements: [
            { name: "Wer sind wir?", hours: 1, enabled: true },
            { name: "Wie sind wir?", hours: 1, enabled: true },
            { name: "Für wen sind wir?", hours: 1, enabled: true },
            { name: "Zielgruppe definieren", hours: 1, enabled: true },
            { name: "Konkurrenzanalyse", hours: 2, enabled: true },
        ],
    },
    {
        name: "Grundbausteine",
        elements: [
            { name: "Farbkonzept", hours: 2, enabled: true },
            { name: "Typografie-System", hours: 2, enabled: true },
            { name: "Logo", hours: 6, enabled: true },
            { name: "Gestaltungskonzept Layout", hours: 3, enabled: true },
            { name: "Brand Guidelines", hours: 3, enabled: true },
            { name: "Icons", hours: 2, enabled: true },
            { name: "Bildwelt definieren", hours: 3, enabled: true },
        ],
    },
    {
        name: "Anwendungen",
        elements: [
            { name: "Web Struktur & Gestaltung", hours: 6, enabled: true },
            { name: "Website umsetzen", hours: 8, enabled: true },
            { name: "Web Unterseiten", hours: 2, enabled: true },
            { name: "Website Testen und optimieren", hours: 3, enabled: true },
            { name: "Portraits fotografieren", hours: 1, enabled: true },
            { name: "Texte schreiben", hours: 3, enabled: true },
            { name: "Bildwelt & 3d Render", hours: 5, enabled: true },
        ],
    },
    {
        name: "Vorlagen*",
        elements: [
            { name: "Offerten", hours: 1, enabled: true },
            { name: "Datenblätter", hours: 1, enabled: true },
            { name: "Visitenkarten, Sticker usw", hours: 1, enabled: true },
            { name: "Rechnungen", hours: 1, enabled: true },
            { name: "Fotostudio einrichten", hours: 2, enabled: true },
            { name: "Bildbearbeitung Vorlagen", hours: 1, enabled: true },
        ],
    },
]);

const seitenzahl = ref(1);
seitenzahl.value= 2;




//data

let today = new Date().toLocaleDateString()

//icons



    
</script>

<style>
/* #region FUNCTIONS */

.column--element.disabled {
    opacity: 0.5;
    transform: scaleX(0.93);
    background-color: azure;
    height: auto;

}

/* #endregion */

/* #region BASESTYLES */

html {
    font-size: 13px;
}

* {
    user-select: none;



    font-family: Arial, Helvetica, sans-serif;
    margin: 0rem;
    padding: 0rem;
}

body {
    padding: 0.625rem 2.5rem 20rem 2.5rem;
}

button {
    border: none;
    margin: 0rem;
    padding: 0rem;
    text-align: inherit;
    font: inherit;
    border-radius: 0rem;

    cursor: pointer;

    background-color: black;
    color: white;

    font-size: 0.875rem;
    padding: 0.375rem;
    border: 0.0625rem solid white;
}

button:hover {
    opacity: 0.8;
}

button.inactive {
    opacity: 0.3;
}

.annotations {
    font-size: 0.6875rem;
    opacity: 0.5;
    position: fixed;
    bottom: 0.625rem;
}

.body--container {
    display: flex;
    flex-direction: column;
    align-items: center;
}

/* #endregion */

/* #region typography */

h1 {
    font-size: 2.75rem;
    font-weight: 900;
    font-family: system-ui, -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, Cantarell, 'Open Sans', 'Helvetica Neue', sans-serif;
    letter-spacing: -0.0625rem;
}

input {
    all: unset;
    font-size: 2.75rem;
    font-weight: 400;
    margin: 0rem;
    padding: 0rem 0.625rem;
    height: fit-content;
    caret-color: green;
    color: #000000;
}

input:focus {
    color: black;
    animation: blinkBorder 1s infinite;
}

@keyframes blinkBorder {
    0%, 100% { color: #4CAF50; }
    50% { color: #68ce6b; }
}

.eyebrow {
    font-size: 0.875rem;
    margin-bottom: 0.625rem;
}

.eyebrow span {
    font-weight: bold;
}

.foothair {
    font-size: 0.875rem;
    position: absolute;
    bottom: 0.625rem;
    right: 1.875rem;
}

.foothair span {
    font-weight: bold;
    font-family: 'Times New Roman', Times, serif;
    font-style: italic;
}



/* #endregion */

/* #region CUSTOM ELEMENTS */

.main--grid {
    display: grid;
    grid-template-columns: 1fr 1fr 1fr 1fr;
    gap: 1rem;
    max-width: 58rem;
}

.column--header {
    font-size: 1rem;
    border-bottom: 0.0625rem solid black;
    padding: 0.25rem;
    margin-bottom: 0.8rem;
}

.column--element {
    display: flex;
    width: 100%;
    align-items: center;
    cursor: pointer;
}

.column--element:hover .element--name {
    border-radius: 0.625rem;
    transform: scale(0.95);
    background-image: linear-gradient(rgba(0, 186, 65, 0.154), rgba(0, 138, 21, 0));
}

.element--name {
    font-size: 0.875rem;
    padding: 1.5rem 1rem;
    text-align: center;
    background-color: rgb(177, 229, 167);
    width: 100%;
    border-bottom: 0.0625rem solid black;
    transition: 0.1s linear transform;
    transition: 0.3s linear border-radius;
}

.element--hours {
    display: flex;
    align-items: center;
    justify-content: center;

    color: white;
    font-size: 0.875rem;

    background-color: black;
    width: 2.125rem;
    height: 2.125rem;
    border-radius: 9.375rem;
    line-height: 0rem;

    aspect-ratio: 1/1;
}

.element--hours p {
    margin: 0rem;
    padding: 0rem;
}

.element--hours:hover{
    transform: scale(1.1);
}

.columnSum {
    display: flex;
    align-items: center;
    justify-content: center;

    color: white;
    font-size: 1rem;
    font-weight: 600;

    background-color: black;
    width: 2.75rem;
    height: 2.75rem;
    border-radius: 9.375rem;
    line-height: 0rem;

    aspect-ratio: 1/1;
}

.macro--controls {
    padding: 0rem;
    position: fixed;
    bottom: 7.25rem;
    left: 10px;
    width: 50rem;
}

.macro--controls .p {
    font-size: 1.5rem;
    font-weight: 800;
}

/* #endregion */

/* #region PREIS SECTION */
.preis--container {
    margin-top: 1.25rem;
    display: grid;
    grid-template-columns: 1fr 1fr 1fr;



    max-width: 600px;


}

.preis--wrapper{
    background-color: rgba(255, 255, 255, 0);
    backdrop-filter: blur(50px);
    position: fixed;
    padding-bottom: 20px;
    bottom: 0px;
    width: 100%;

}

.preis--container p {
    font-weight: 600;
    font-size: 0.875rem;
    border-bottom: 0.0625rem solid black;
    padding: 0rem 0rem 0.1875rem 0rem;
}
/* #endregion */

.low-hours {
    background-color: rgb(177, 229, 167);
    padding: 0.8rem 1rem;
}

.medium-hours {
    background-color: #88d48a;
    padding: 1.4rem 1rem;
}

.high-hours {
    background-color: #4eb551;
    padding: 1.9rem 1rem;
}
</style>
