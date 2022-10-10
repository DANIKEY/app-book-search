<template>
  <div>
     <div class="q-pa-md row items-start q-gutter-md"> 
         <div  v-for="book in books" :key="book.id" class="col-3">
            <q-card class="my-card">
          <q-img src="https://cdn.quasar.dev/img/parallax2.jpg">
            <div class="absolute-bottom">
              <div class="text-h6">{{book.title}}</div>
              <div class="text-h6">{{book.kind_of_book}}</div>
              <div class="text-h6">{{book.price}}€</div>
              <div class="text-subtitle2">par {{ book.author}}</div>
            </div>
          </q-img>
        </q-card>
        </div>
     </div>
  </div>  
</template>

<script>
import { defineComponent} from 'vue';
export default defineComponent({
    props: {
       books: {
        type: Array
      },
    }
})
</script>

<style>
.my-card {
  width: 100%,
}
</style>

