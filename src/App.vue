<template>
  <div class="h-screen flex flex-col justify-center items-center bg-gray-50">
    <div class="p-3 w-full h-full md:h-fit md:w-1/2 lg:w-1/3 md:rounded-2xl md:border-2 border-gray-200 mb-5">
      <h1 class="text-2xl text-center text-gray-700 mb-2 font-bold">Calculate BMR</h1>
      <form @submit.prevent="calculateBMR">
        <div class="mb-3">
          <label for="gender">Gender</label>
          <select
            id="gender"
            v-model="gender"
            class="bg-gray-50 border border-gray-300 text-gray-900 text-sm rounded-lg block w-full p-2.5"
          >
            <option value="male">Male</option>
            <option value="female">Female</option>
          </select>
        </div>

        <div class="mb-3">
          <label for="age">Age (y)</label>
          <input
            v-model.number="age"
            type="number"
            id="age"
            name="age"
            placeholder="Enter your age"
            required
            min="0"
            class="bg-gray-50 border border-gray-300 text-gray-900 text-sm rounded-lg block w-full p-2.5"
          />
        </div>

        <div class="mb-3">
          <label for="weight">Weight (kg)</label>
          <input
            v-model.number="weight"
            type="number"
            id="weight"
            name="weight"
            placeholder="Enter your weight"
            required
            min="0"
            class="bg-gray-50 border border-gray-300 text-gray-900 text-sm rounded-lg block w-full p-2.5"
          />
        </div>

        <div class="mb-5">
          <label for="height">Height (cm)</label>
          <input
            v-model.number="height"
            type="number"
            id="height"
            name="height"
            placeholder="Enter your height"
            class="bg-gray-50 border border-gray-300 text-gray-900 text-sm rounded-lg block w-full p-2.5"
            required
            min="0"
          />
        </div>

        <button
          type="submit"
          class="bg-gray-50 border border-gray-300 text-gray-900 text-sm rounded-lg block w-full p-2.5 hover:bg-gray-100"
        >
          Calculate BMR
        </button>
      </form>
    </div>

    <div class="p-3 w-full md:w-1/2 lg:w-1/3 md:rounded-2xl md:border-2 border-gray-200">
      Result: {{ bmr }} {{ unit }}
    </div>
  </div>
</template>

<script>
import axios from "axios";

export default {
  name: "App",
  data() {
    return {
      gender: "male",
      age: null,
      weight: null,
      height: null,
      bmr: null,
      unit: null,
      interpretation: null,
    };
  },
  methods: {
    calculateBMR() {
      // call Web API
      this.fetchData();
    },
    fetchData() {
      let url = "https://pirun.ku.ac.th/~faaspsu/edu/mobile/calculatebmr.php";
      let params =
        "?weight=" +
        this.weight +
        "&height=" +
        this.height +
        "&age=" +
        this.age +
        "&gender=" +
        this.gender;
      axios
        .get(url + params)
        .then((response) => {
          console.log(response.data.bmrdata);
          this.bmr = response.data.bmrdata.bmr;
          this.unit = response.data.bmrdata.unit;
          this.interpretation = response.data.bmrdata.interpretation;
        })
        .catch((error) => {
          console.log("Error fetching data: ", +error);
        });
    },
  },
};
</script>

<style>
</style>
