<script setup lang="ts">
import NewDialogComponent from "../components/NewDialogComponent.vue";
import { useStore } from "src/stores/store";
import { ref, onMounted, watch } from "vue";
const store = useStore();
const selectedCategoryName = ref("Személyautó");
let toggled = ref(false);

const longText = ref("");
let displayText = ref("");
onMounted(() => {
  store.getAllCategories();
  fetchData();
});

watch(selectedCategoryName, () => {
  fetchData();
});

const fetchData = () => {
  store.one_GetByCategory(selectedCategoryName.value?.toString() || "Személyautó");
};

const formatPrice = (price: number | string | undefined): string => {
  if (!price) {
    return "";
  }
  const ubreakableSpace = "\u00A0";
  const p = price?.toString();
  let result = "";
  for (let i = p.length - 1; i >= 0; i--) {
    result = p[i] + result;
    if ((result.length + 1) % 4 == 0) {
      result = ubreakableSpace + result;
    }
  }

  return result + ubreakableSpace + "Ft";
};

const openDialog = (item) => {
  store.many.document = item;
  store.app.showNewDialog = true;
};

const doToggle = (toggled) => {
  if (!toggled) {
    let slice = "";
    let lastindex = -1;
    for (let index = 0; index < 100; index++) {
      slice += longText.value[index];
      if (longText.value[index] == " ") {
        lastindex = index;
      }
    }
    if (slice[99] == " ") {
      displayText.value = slice.slice(0, 98) + "...";
    } else {
      displayText.value = slice.slice(0, lastindex) + "...";
    }
  } else {
    displayText.value = longText.value;
  }
};
</script>
<template>
  <q-page>
    <div class="row justify-center">
      <q-select
        v-model="selectedCategoryName"
        emit-value
        label="Kategória"
        map-options
        option-label="categoryNameField"
        option-value="id"
        :options="store.many.documents"
        :rules="[(v) => v != null || 'Kérem válasszon kategóriát!']"
      ></q-select>
    </div>
    <NewDialogComponent />
    <div class="row justify-center q-ma-xl">
      <div v-for="(item, index) in store.many.cars" :key="index" class="col-xs-12 col-sm-12 col-md-2 col-lg-2">
        <q-card bordered class="q-ma-md" flat>
          <q-card-section class="text-center text-h5" style="background-color: rgb(200, 190, 150)">
            {{ item.cim }} - {{ formatPrice(item.vetelar) }}
          </q-card-section>
          <q-card-section class="text-h7" style="background-color: rgb(250, 225, 200)">
            <ul>
              <li>
                <span>Szin:</span>
                <b>{{ item.szin }}</b>
              </li>
              <li>
                <span>Évjárat:</span>
                <b>{{ item.evjarat }}</b>
              </li>
              <li>
                <span>Hengerűrtartalom:</span>
                <b>{{ item.hengerurtartalom }} cm<sup>2</sup></b>
              </li>
              <li>
                <span>Hirdetés dátuma:</span>
                <b>{{ item.hirdetes_datum }}</b>
              </li>
            </ul>
          </q-card-section>
          <q-card-section class="" style="background-color: rgb(200, 190, 156)">
            <div class="text-h7 text-justify">{{ item.leiras }}</div>
            <hr />
            <q-toggle
              v-model="toggled"
              color="dark-blue"
              :disable="longText.length <= 100"
              label="Teljes hirdetés"
              @update:model-value="doToggle"
            />
          </q-card-section>
          <q-card-section style="background-color: beige">
            <q-img role="img" :src="item.kepek?.[0]" style="max-height: 200px"></q-img>
          </q-card-section>
          <q-card-section style="background-color: chocolate">
            <div class="text-h7 text-justify">{{ item.kepek?.[0] }}</div>
          </q-card-section>
          <q-card-actions class="justify-center" style="background-color: whitesmoke">
            <q-btn
              class="bg-green-5"
              label="Hirdetés szerkesztése"
              no-caps
              type="button"
              @click="openDialog(item)"
            ></q-btn>
          </q-card-actions>
        </q-card>
      </div>
    </div>
  </q-page>
</template>

<style lang="scss" scoped></style>
