<script lang="ts">
import { Value } from '@/scripts/value'

export default {
  props: ['values', 'value_types'],
  data() {
    return {
      selectedValueType: '', // Initialize with an empty string for no filtering
    };
  },
  // ...
}
</script>
