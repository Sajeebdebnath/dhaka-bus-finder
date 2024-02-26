<script setup>
import { ref, computed } from "vue";

const origin = ref("");
const destination = ref("");
const availableBuses = ref([]);

const areaList = [
  "Farmgate",
  "Press Club",
  "Paltan",
  "Mirpur-1",
  "Mirpur-2",
  "Mirpur-10",
  "Shewrapara",
  "Kazipara",
  "Agargaon",
  "Mohammadpur",
];

const busList = [
  {
    busName: "Shikhor",
    origin: "Mirpur",
    destination: "Gulistan",
    stops: ["Farmgate", "Press Club", "Paltan"],
  },
  {
    busName: "Ayat",
    origin: "Mohakhali",
    destination: "Sayedabad",
    stops: ["Tejgaon", "Farmgate", "Paltan"],
  },
];

const filteredDestinations = computed(() => {
  if (origin.value) {
    return areaList.filter((station) => station !== origin.value);
  } else {
    return areaList;
  }
});

const filteredOrigins = computed(() => {
  if (destination.value) {
    return areaList.filter((station) => station !== destination.value);
  } else {
    return areaList;
  }
});

const findAvailableBuses = () => {
  if (origin.value && destination.value) {
    const availableList = busList.filter((bus) => {
      return (
        bus.stops.includes(origin.value) &&
        bus.stops.includes(destination.value)
      );
    });
    availableBuses.value = availableList;
  } else {
    availableBuses.value = [];
  }
};
</script>

<template>
  <div class="hero-area">
    <div class="container h-100">
      <div class="row algin-center h-100">
        <div class="col-lg-5 col-md-7">
          <div class="bus-stoppage-form">
            <form name="form">
              <div class="form-group">
                <label for="origin">মূল স্টেশন :</label>
                <input
                  type="text"
                  v-model="origin"
                  placeholder="Type Origin"
                  @input="findAvailableBuses"
                />
                <ul class="dropdown-list">
                  <li
                    v-for="station in filteredOrigins"
                    :key="station"
                    @click="origin = station"
                  >
                    {{ station }}
                  </li>
                </ul>
              </div>
              <div class="form-group">
                <label for="destination">গন্তব্য স্টেশন :</label>
                <input
                  type="text"
                  v-model="destination"
                  placeholder="Type Destination"
                  @input="findAvailableBuses"
                />
                <ul class="dropdown-list">
                  <li
                    v-for="station in filteredDestinations"
                    :key="station"
                    @click="destination = station"
                  >
                    {{ station }}
                  </li>
                </ul>
              </div>
            </form>
            <template v-if="availableBuses.length > 0">
              <h2>Available Buses:</h2>
              <ul>
                <li v-for="bus in availableBuses" :key="bus.busName">
                  {{ bus.busName }}
                </li>
              </ul>
            </template>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped></style>
