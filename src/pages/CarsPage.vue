<template>
    <q-page>
      <q-container>
        <q-card
          v-for="(car, index) in cars"
          :key="index"
          class="q-mb-md"
        >
          <q-card-section>
            <q-item>
              <q-item-section>
                <q-item-label>{{ car.make }} {{ car.model }}</q-item-label>
                <q-item-label caption>{{ car.year }}</q-item-label>
              </q-item-section>
              <q-item-section side>
                <q-btn
                  icon="edit"
                  @click="editCar(index)"
                  class="q-mr-sm"
                />
                <q-btn
                  icon="delete"
                  @click="deleteCar(index)"
                />
              </q-item-section>
            </q-item>
          </q-card-section>
        </q-card>
      </q-container>
    </q-page>
  </template>
  
  <script>
  export default {
    data() {
      return {
        cars: [
          { make: "Toyota", model: "Corolla", year: 2022 },
          { make: "Honda", model: "Civic", year: 2021 },
          { make: "Ford", model: "Mustang", year: 2020 }
          // Add more cars as needed
        ]
      };
    },
    methods: {
      editCar(index) {
        // Handle editing of car at the given index
        console.log('Editing car at index', index);
      },
      deleteCar(index) {
        // Handle deletion of car at the given index
        console.log('Deleting car at index', index);
        this.cars.splice(index, 1);
      }
    }
  };
  </script>
  
  <style scoped>
  /* Add custom styles here */
  </style>
  