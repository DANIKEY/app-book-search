<template>
  <q-page class="row items-center justify-evenly">
     <search-component></search-component>
  </q-page>
</template>

<script lang="ts">
import { defineComponent, ref } from 'vue';
import SearchComponent from 'components/SearchComponent.vue';
export default defineComponent({
  name: 'PageIndex',
  components: {SearchComponent},
  setup() {}
});
</script>
