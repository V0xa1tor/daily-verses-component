<script setup lang="ts">
// https://dailyverses.net

const bibleText = ref("");
const bibleVerse = ref("");

enum verseUrl {
  daily = "https://dailyverses.net/get/verse.js",
  random = "https://dailyverses.net/get/random.js"
};

enum verseLang {
  ARA = "ara",
  ARC = "arc",
  NVI = "nvi-pt"
};

type typeT = "daily" | "random";
type langT = "ARA" | "ARC" | "NVI";
const selectedType = ref<typeT>("daily");
const selectedLang = ref<langT>("NVI");
// Set localStorage
// watch(selectedType, value => localStorage.setItem("verseType", value));
// watch(selectedLang, value => localStorage.setItem("verseLang", value));

// Append script on body and returns it
function loadScript(src: string) {
  const script = document.createElement("script");
  script.async = true;
  script.src = src;
  return document.body.appendChild(script);
}

function loadVerse() {
  bibleText.value = "";
  bibleVerse.value = "";
  // Usage: [URL]?language=[lang]
  // Example: https://dailyverses.net/get/verse.js?language=nvi-pt
  loadScript(`${verseUrl[selectedType.value]}?language=${verseLang[selectedLang.value]}`);
}

onMounted(() => {
  // Load localStorage
  // selectedType.value = (localStorage.getItem("verseType") ?? "daily") as typeT;
  // selectedLang.value = (localStorage.getItem("verseLang") ?? "NVI") as langT;
  loadVerse();

  const dailyVersesWrapper = document.getElementById('dailyVersesWrapper')!;
  
  // Extract text
  const observer = new MutationObserver(([e]) => {
    const [text, verse] = [...e.addedNodes].map(n => n.textContent ?? "");
    bibleText.value = text;
    bibleVerse.value = verse;
  });
  observer.observe(dailyVersesWrapper, { childList: true });
  
});

</script>

<template>
  <div>
    <div class="row g-2 mb-2">
      <div class="col-auto">
        <div class="input-group">
          <select @change="loadVerse" v-model="selectedType" class="form-select">
            <option value="daily">Versículo diário</option>
            <option value="random">Versículo aleatório</option>
          </select>
          <button v-if="selectedType == 'random'" class="btn btn-outline-secondary" @click="loadVerse" title="Gerar outro"><i class="bi bi-dice-3"></i></button>
        </div>
      </div>
      <div v-if="!bibleText || !bibleVerse" class="col-auto d-flex align-items-center">
        <div class="spinner-border border-2" role="status">
          <span class="visually-hidden">Loading...</span>
        </div>
        <!-- <select @change="loadVerse" v-model="selectedLang" class="form-select">
          <option value="ARA">ARA</option>
          <option value="ARC">ARC</option>
          <option value="NVI">NVI</option>
        </select> -->
      </div>
    </div>

    <div hidden id="dailyVersesWrapper"></div>
    <div id="MyDailyVerses">
      <div v-if="bibleText" class="bible-text fs-4">{{ bibleText }}</div>
      <!-- <div v-else class="fs-4">
        <span class="placeholder col-12"></span>
        <span class="placeholder col-8"></span>
      </div> -->
      <small v-if="bibleVerse" class="bible-verse text-body-secondary">{{ bibleVerse }}</small>
      <!-- <div v-else>
        <span class="text-body-secondary placeholder placeholder-sm" style="width: 120px;"></span>
      </div> -->
    </div>

  </div>
</template>

<style>
.bible-text {
  font-family: serif;
}
.bible-verse {
  font-family: monospace;
}
</style>
