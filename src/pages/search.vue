<template>
  <q-page>
      <!--{{books}} -->
       <div v-if="searchBooks.length != 0">
         <!-- {{searchBooks}} -->
         <book-component :books="searchBooks"></book-component>
      </div>
      <div v-else>
         Aucun livre ne correspond à  votre recherche
      </div> 
  </q-page>
</template>

<script>
import { defineComponent, ref } from 'vue';
import  ApiBooks from '../services/BookService';
import BookComponent from 'components/BookComponent.vue';

export default defineComponent({
    components: { BookComponent },
    data(){
      return {
        books: [],
        search: this.$route.params.search
      }
    }, 
    computed : {
     // filter books data 
      searchBooks(){
       //return this.books.filter(fetch => fetch.title.toLowerCase() == this.search.toLowerCase())
       return this.books.filter(fetch => fetch.title.toLowerCase().indexOf(this.search.toLowerCase()) !== -1)
     }
    },
    mounted(){

      //alert(this.search)
      //get data books 
       this.getAllBooks()
    },
    methods : {
     
      //get all books 
      getAllBooks(){
        ApiBooks.getBooks()
        .then((response)=> {
         //console.log(response)
          this.books = response
          this.searchBooks
        })
        .catch(e => {e})
      },
    },
    setup() {},
})
</script>
