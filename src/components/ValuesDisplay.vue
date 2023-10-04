<template>
  <div>
    <!-- Dropdown menu for filtering -->
    <label for="valueTypeDropdown">Filter by Value Type:</label>
    <select id="valueTypeDropdown" v-model="selectedValueType">
  <option value="">All</option>
  <option value="Temp">Temp</option>
  <option value="Humidity">Humidity</option>
  <option value="Airpressure">Airpressure</option>
</select>


    <!-- Value display using filteredValues -->
    <div class="row bg-primary mt-2 mb-1">
      <div
        class="col-1"
        data-bs-toggle="tooltip"
        data-bs-placement="top"
        data-bs-title="Tooltip on top"
      >
        time (UTC)
      </div>
      <div class="col-1">type</div>
      <div class="col">value</div>
    </div>
    <div class="row bg-secondary rounded mt-1" v-for="value in filteredValues" :key="value">
      <div class="col-1">
        {{ formatDateFromTimestamp(value.time) }}
      </div>
      <div class="col-1">
        {{ getTypeName(value) }}
      </div>
      <div class="col">{{ value.value.toFixed(2) }} {{ getUnit(value) }}</div>
    </div>
  </div>
</template>



<script lang="ts">
import { Value } from '@/scripts/value'
import { ref, computed } from 'vue'

export default {
  props: ['values', 'value_types', 'valueTypes'],
  setup(props) {
    const selectedValueType = ref('') // Initialize with an empty string for no filtering

    const getTypeName = (value: Value) => {
      for (var i = 0; i < props.value_types.length; i++) {
        if (props.value_types[i].id == value.value_type_id) {
          return props.value_types[i].type_name
        }
      }
      return 'XXX'
    }

    const getUnit = (value: Value) => {
      for (var i = 0; i < props.value_types.length; i++) {
        if (props.value_types[i].id == value.value_type_id) {
          return props.value_types[i].type_unit
        }
      }
      return 'XXX'
    }

    const formatDateFromTimestamp = (timestamp: any) => {
      const date = new Date(timestamp * 1000); // Convert timestamp to milliseconds
      return date.toISOString().slice(0, 19).replace("T", " ");
    }

    // Computed property for filtered values
    // Computed property for filtered values
const filteredValues = computed(() => {
  if (!selectedValueType.value) {
    // If no value type is selected, return all values
    return props.values;
  }

  // Filter values by the selected value type name
  return props.values.filter(value => getTypeName(value) === selectedValueType.value);
})


    return {
      selectedValueType,
      getTypeName,
      getUnit,
      formatDateFromTimestamp,
      filteredValues
    }
  },
  emits: ['rename'],
}
</script>

