<template>
    <div> 
      <span style="position: relative; display: inline-block">
      <div class="bloc-recherche" style="width:450px">
        <input 
          id="search" 
          type="text" 
          autocomplete="off" 
          v-model.lazy="search" 
          @input="update" 
          v-on:keyup="keymonitor" 
          class="input-recherche"  
          placeholder="Rechercher un livre par le titre..." 
          required>    
      </div>
    </span>

    <q-icon id="search-btn-icon" @click="searchRequest()" name="search"><img src="~assets/img/search.svg" alt=""/></q-icon>

      <div style="display:flex">
         <div style="width:100%;">
           <div id="results" class="resultat"></div>
        </div>
        <div style="width:100%"></div>
      </div>
    </div>
</template>
<script>
import { defineComponent, ref } from 'vue';
import  ApiBooks from '../services/BookService';

export default defineComponent({
  data(){
    return{
     search : '',
     books: [],

     previousRequest: '',
     selectedResult: -1,
     selectedResultMobile: -1,
    }
  },
  mounted(){
    //get data books 
    this.getAllBooks()
  },
  methods:{
     //get all books 
     getAllBooks(){
      ApiBooks.getBooks()
      .then((response)=> {
        this.books = response
      })
      .catch(e => {e})
    },
    
    // search event redirection
    searchRequest()
    {
      let val = this.search
      if(val != "")
      { 
        this.$router.push('/recherche/'+this.search)
      }
      else{
        alert("Rechercher un livre par le titre..")
      }
      
    },

    // display autocomplete results

    displayResults(response) {
        console.log(response)
        var results = document.getElementById('results')
        var searchElement = document.getElementById('search')
        results.style.display = response.length ? 'block' : 'none';
        if (response.length) {
          var responseLen = response.length;
          results.innerHTML = '';
          for (var i = 0, div; i < responseLen; i++) {
            div = results.appendChild(document.createElement('div'));
            div.innerHTML = response[i].title;
            div.addEventListener('click', function(e) {
              searchElement.value = e.target.innerHTML
              this.search = searchElement.value
              results.style.display = 'none'
              e.target.className = '';
              this.selectedResult = -1
              searchElement.focus()
            });
          }
        }
    },

    // update autocomplete data
    update(e) {
      let searchElement = document.getElementById('search').value

      this.displayResults(this.filterRequest(this.books,searchElement))
    },
    // keycode events selections
    keymonitor: function(event) {

     //console.log(event.keyCode)

     var results = document.getElementById('results')

     var divs = results.getElementsByTagName('div')


     if (event.keyCode == 38 && this.selectedResult > -1) {

      divs[this.selectedResult--].className = '';

     if (this.selectedResult > -1) {

      divs[this.selectedResult].className = 'autocomplete-active';

      console.log(divs[this.selectedResult].innerHTML)

      this.search = divs[this.selectedResult].innerHTML

      document.getElementById("search").value =  divs[this.selectedResult].innerHTML

    }
    } else if (event.keyCode == 40 && this.selectedResult < divs.length - 1) {

     results.style.display = 'block';

    if(this.selectedResult > -1) {

     divs[this.selectedResult].className = '';
    }

    divs[++this.selectedResult].className = 'autocomplete-active';
     
    console.log(divs[this.selectedResult].innerHTML)
    
    document.getElementById("search").value =  divs[this.selectedResult].innerHTML
    this.search = divs[this.selectedResult].innerHTML
   } else if (event.keyCode == 13) { 
    
     event.preventDefault()
     //document.getElementById("search-btn-icon").click();
     this.searchRequest()
   }

  },

  // filter title autocomplete
  filterRequest(arr, requete) {
    return arr.filter(fetch => fetch.title.toLowerCase().indexOf(requete.toLowerCase()) !== -1  )
  },

  // choose autocomplete results 
  chooseResult(result) {

    var searchElement = document.getElementById('search')

    var results = document.getElementById('results')

    this.search = result.innerHTML

    results.style.display = 'none'

    result.className = '';

    this.selectedResult = -1

    searchElement.focus()
  },
 }

})

</script>
