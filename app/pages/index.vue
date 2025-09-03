<template>
    <div>
        <p class="eyebrow"> <span>CLIMAEXPERT </span>Branding & Webdesign </p>
        <div style="display:flex">

            <h1 style="margin-bottom: 28px;">Offerte |</h1> <input type="text" placeholder="Projektname"> 
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
      @click="toggleElement(column, element)"
      :class="{ disabled: !element.enabled }"
    >
      <p class="element--name">{{ element.name }}</p>
      <div class="element--hours">
        <p>{{ getEffectiveHours(element) }}h</p>
      </div>
    </div>

    <!-- Arrow + column total at bottom -->
    <div
      style="width: calc(100% - 34px); display: flex; flex-direction: column; align-items: center; gap: 10px;"
    >
      <svg
        style="width: 30px;"
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


        <section class="preis--container">
            <div>
                <p>Stundenaufwand</p>
                <h1>{{ totalHours }}h</h1>
            </div>
            <div :style="{ opacity: columnSum > 2000 ? 0.2 : 1 }">
                <p >Preis</p>
                <h1>
                {{ columnSum }}.-</h1>
            </div>
            <div :style="{ opacity: columnSum < 2000 ? 0.2 : 1 }">
                <p>Preis mit Grössenrabatt*</p>

                <h1
                v-if="columnSum<2000"
                >ohne</h1>
                <h1
                v-else
                >{{ Math.floor((columnSum * 0.85)/10) *10}}.-</h1>
            </div>
        </section>
        <p class="annotations">*Weil grössere Aufträge viel Zeit in Administration und Akquise sparen, können sie kostengünstiger angeboten werden.</p>
    </div>





 </section>
</section>

<p class="foothair"> 03.09.25 <span>&pfeifer </span> </p>

</template>

<script setup>
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


const stundenLohn = 150;

const columns = ref([
    {
        name: "Research / Marketing",
        elements: [
            { name: "Wer sind wir?", hours: 1, enabled: true },
            { name: "Wie sind wir?", hours: 0.5, enabled: true },
            { name: "Für wen sind wir?", hours: 0.5, enabled: true },
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
            { name: "Gestaltungs-Elemente (Icons etc)", hours: 2, enabled: true },
            { name: "Bildwelt definieren", hours: 3, enabled: true },
        ],
    },
    {
        name: "Anwendungen",
        elements: [
            { name: "Website Struktur & Gestaltung", hours: 4, enabled: true },
            { name: "Website umsetzen", hours: 6, enabled: true },
            { name: "Website Unterseiten umsetzen", hours: 5, enabled: true },
            { name: "Website Testen und optimieren", hours: 3, enabled: true },
            { name: "Portraits fotografieren", hours: 2, enabled: true },
            { name: "Texte schreiben", hours: 3, enabled: true },
            { name: "Bildwelt fotografieren & 3d Render", hours: 5, enabled: true },
        ],
    },
    {
        name: "Vorlagen*",
        elements: [
            { name: "Offerten", hours: 2, enabled: true },
            { name: "Datenblätter", hours: 2, enabled: true },
            { name: "Visitenkarten, Sticker usw", hours: 2, enabled: true },
            { name: "Rechnungen", hours: 1, enabled: true },
            { name: "Fotostudio einrichten", hours: 3, enabled: true },
            { name: "Bildbearbeitung Vorlagen", hours: 3, enabled: true },
        ],
    },
]);

const seitenzahl = ref(1);
seitenzahl.value= 1.5;
    
</script>

<style>
/* #region FUNCTIONS */

.column--element.disabled {
    opacity: 0.5;
    transform: scaleX(0.93);
    background-color: azure;

}


/* #endregion */

/* #region BASESTYLES */



    * {
        font-family: Arial, Helvetica, sans-serif;
        margin: 0px;
        padding: 0px;
    }

    body {
        padding: 10px 40px 20px 40px;
    }


    button {
        border: none;
        margin: 0;
        padding: 0;
        text-align: inherit;
        font: inherit;
        border-radius: 0;

        cursor: pointer;

        background-color: black;
        color: white;

        font-size: 14px;
        padding: 6px;
        border: 1px solid white;
    }

    button:hover {
        opacity: .8;
    }

    button.inactive {
        opacity: 0.3;
    }

    .annotations {
        font-size: 11px;
        opacity: 0.5;
        position: fixed;
        bottom: 10px;
    }

    .body--container{
        display: flex;
        flex-direction: column;
        align-items: center;
    }
    /* #endregion */



/* #region typography */

    h1 {
        font-size: 44px;
        font-weight: 900;
        font-family:system-ui, -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, Cantarell, 'Open Sans', 'Helvetica Neue', sans-serif;
        letter-spacing: -1px;
    }

    input {
        all: unset;
        font-size: 44px;
        font-weight: 900;
        font-family:system-ui, -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, Cantarell, 'Open Sans', 'Helvetica Neue', sans-serif;
        letter-spacing: -1px;
        margin: 0px;
        padding: 0px 10px 0px 10px; 
        height: fit-content;
        caret-color: green;
        color: #4CAF50;

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
        font-size: 14px;
        margin-bottom: 10px;
    }

    .eyebrow span {
        font-weight: bold;
        
    }

    .foothair {
        font-size: 14px;
        position: absolute;
        bottom: 10px;
        right: 30px;
    }

    .foothair span {
        font-weight: bold;
        font-family: 'Times New Roman', Times, serif;
        font-style: italic;
        
    }


/* #endregion */


/* #region CUSTOM ELEMENTS */

    .main--grid{
        display: grid;
        grid-template-columns: 1fr 1fr 1fr 1fr;
        gap: 16px;
        max-width: 940px;
    }

    .column--header{
        font-size: 16px;
        border-bottom: 1px solid black;
        padding: 4px;
        margin-bottom: 16px;
        
    }

    
    .column--element{
        display: flex;
        width: 100%;
        align-items: center;
        cursor: pointer;

    }

    .column--element:hover .element--name {
        border-radius: 10px;
        transform: scale(0.95);
        background-image: linear-gradient(rgba(0, 186, 65, 0.154), rgba(0, 138, 21, 0));
        ;
    }

    .element--name{
        font-size: 14px;
        padding: 24px 16px;
        text-align: center;
        background-color: rgb(177, 229, 167);
        width: 100%;
        border-bottom: 1px solid black;
        transition: .1s linear transform;
        transition: .3s linear border-radius;


    }

    .element--hours {
        display: flex;
        align-items: center;
        justify-content: center;

        color: white;
        font-size: 14px;

        background-color: black;
        width: 34px;
        height: 34px;
        border-radius: 150px;
        line-height: 0px;

        aspect-ratio: 1/1;
        


    }

    .element--hours p {
        margin: 0px;
        padding: 0px;
    }


    .columnSum {
        display: flex;
        align-items: center;
        justify-content: center;

        color: white;
        font-size: 16px;
        font-weight: 600;


        background-color: black;
        width: 44px;
        height: 44px;
        border-radius: 150px;
        line-height: 0px;

        aspect-ratio: 1/1;
    }
    




    .macro--controls{
        padding: 0px 0px;
        position: fixed;
        bottom: 36px;
        width: 800px;
    }

    .macro--controls .p{
        font-size: 24px;
        font-weight: 800;
    }

    /* #endregion */


/* #region PREIS SECTION */
.preis--container {
        margin-top: 20px;

        display: grid;
        grid-template-columns: 1fr 1fr 1fr;

    }

    .preis--container p {
        font-weight: 600;
        font-size: 14px;
        border-bottom: 1px solid black;
        padding: 0px 0px 3px 0px
    }

    /* #endregion */


</style>