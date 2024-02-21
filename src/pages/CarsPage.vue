<template>
  <q-page>
    <q-container>
      <q-card
        v-for="(car, index) in cars"
        :key="index"
        class="q-mb-md"
      >
        <q-card-section>
          <div class="text-h6">{{ car.make }} {{ car.model }}</div>
          <div class="text-subtitle1">Price: {{ car.price }}</div>
          <ul class="q-mt-sm">
            <li>Color: {{ car.color }}</li>
            <li>Year: {{ car.year }}</li>
            <li>Displacement: {{ car.displacement }}</li>
            <li>Ad posted on: {{ car.adPosted }}</li>
          </ul>
          <q-toggle
            v-if="car.description.length > 150"
            v-model="showFullDescription[index]"
            class="q-mt-sm"
            label="Show Full Description"
            
          />
          <div v-if="showFullDescription[index] || car.description.length <= 150" class="q-mt-sm">{{ car.description }}</div>
          <div class="q-mt-sm">
            <q-img
              v-for="(image, imgIndex) in car.images.slice(0, 3)"
              :key="imgIndex"
              class="q-mr-sm"
              :src="image"
              style="max-width: 100px; max-height: 100px;"
            />
          </div>
          <q-btn
            class="q-mt-md"
            label="Edit Car"
            @click="editCar(index)"
          />
        </q-card-section>
      </q-card>
    </q-container>
  </q-page>
</template>

<script>
export default {
  methods: {
    editCar(index) {
      // Handle editing of car at the given index
      console.log('Editing car at index', index);
    }
  }
};
</script>

<style scoped>
/* Add custom styles here */
</style>
