<script setup lang="ts">
import { useStore } from "../stores/store";
import { Dialog } from "quasar";
const store = useStore();

function ShowDialog() {
  // kategórák kiolvasása az "egy" oldalról
  store.one_GetAll();
}

function HideDialog() {
  store.many.document = {};
}

function Close() {
  store.app.showNewDialog = false;
}

function Reset() {
  store.many.document = { ...store.many.documentOld };
}
function Submit() {
  Dialog.create({
    title: "Megerősítés",
    message: "Szeretnéd menteni a változásokat?",
    ok: "Igen",
    cancel: "Nem",
    persistent: true,
  })
    .onOk(() => {
      store.many_EditById();
      Close();
    })
    .onCancel(() => {
      Close();
    });
}
</script>

<template>
  <q-dialog v-model="store.app.showNewDialog" persistent @hide="HideDialog()" @show="ShowDialog()">
    <q-card class="q-pa-md q-gutter-md" style="width: 60vw; min-width: 300px">
      <q-form @submit="Submit()">
        <div class="q-gutter-md">
          <h5 class="text-center">Hirdetés szerkesztése</h5>
          <q-select
            v-model="store.many.document.categoryId"
            emit-value
            label="Ketegória"
            map-options
            option-label="categoryNameField"
            option-value="id"
            :options="store.one.documents"
            :rules="[(v) => v != null || 'A kategória nem lehet üres']"
            type="text"
          />
          <q-input
            v-model="store.many.document.cim"
            filled
            label="Cím"
            :rules="[(v) => (v != null && v != '') || 'Kérem válasszon kategóriát!']"
            type="text"
          />
          <q-input
            v-model="store.many.document.leiras"
            filled
            label="Leírás"
            :rules="[(v) => (v != null && v != '') || 'Kérem töltse ki a leírást!']"
            type="textarea"
          />
          <q-input v-model="store.many.document.hirdetes_datum" filled label="Hirdetés dátuma" type="date" />
          <q-checkbox v-model="store.many.document.serulesmentes" label="Sérülésmentes" />
          <q-input
            v-model="store.many.document.vetelar"
            filled
            label="Meghirdetett ár"
            :rules="[(v) => (v != null && v != '') || 'Kérem adja meg az árat!']"
            type="text"
          />
          <q-input
            v-model="store.many.document.kepek"
            filled
            label="Fotó(k) az eladó járműről"
            :rules="[(v) => (v != null && v != '') || 'Kérem adja meg az árat!']"
            type="text"
          />

          <div class="row justify-center q-gutter-lg">
            <q-btn color="green" label="Save" no-caps type="submit" />
            <q-btn color="red" label="Reset" no-caps type="reset" @click="Reset()" />
            <q-btn color="blue" label="Close" no-caps @click="Close()" />
          </div>
        </div>
      </q-form>
    </q-card>
  </q-dialog>
</template>

<style lang="scss" scoped></style>
