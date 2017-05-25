<template>
  <div>

    <br><br>
    <div v-if="showAllBooks">
      <form v-on:submit.prevent="searchBooks">
            <input
              placeholder="Search books"
              v-model="query"
              class= "button-style"
            ></input>
          <button class="button-style" type="submit">Search</button>
      </form>
    </div>
    <div v-if="showOneBook">
      <button style="margin-left: -52%;" class="button-style" @click="returnBack">Return Back</button>
    </div>
    <br><br>

    

<div class="container" v-if="showAllBooks">
  <div class="row">
    <div class="col-md-4" v-for="book in books">
      <figure class="book-fig" @click="showBook(book)">
      <img v-if="book.volumeInfo.imageLinks.thumbnail" :src="book.volumeInfo.imageLinks.thumbnail" alt="profile-sample2" style="width: 300px; height: 250px;"/>
        <figcaption>
          <h4 class="title1">{{ book.volumeInfo.title }}</h4>
          <h4 class="title3" v-if="book.volumeInfo.publisher">{{ book.volumeInfo.publisher }}</h4>
        </figcaption><a href="#"></a>
      </figure>
    </div>
  </div>
</div>

<div class="container" v-if="showOneBook">
  <div class="row">
    <div class="col-md-6">
      <figure class="book-fig">
      <img v-if="chosenBook.volumeInfo.imageLinks.thumbnail" :src="chosenBook.volumeInfo.imageLinks.thumbnail" alt="profile-sample2" style="width: 1000px; height: 500px;"/>
        <figcaption>
        </figcaption><a href="#"></a>
      </figure>
    </div>
    <div class="col-md-6">
      <h1 id="title1">{{ chosenBook.volumeInfo.title }}</h1>
      <h2 id="title1">BY</h2>
      <h2 id="title2" v-for="author in chosenBook.volumeInfo.authors">{{ author }}</h2>
      <h3 id="title3" v-if="chosenBook.volumeInfo.publisher">PUBLISHER: {{ chosenBook.volumeInfo.publisher }}</h3><br>
      <h4 id="title3" align="left" style="text-decoration: underline;">BOOK DESCRIPTION:</h4>
      <p id="paragraph">{{ chosenBook.volumeInfo.description }}</p>
    </div>
  </div>
</div>



  </div>
</template>

<script>

  export default {
    /**
     * The name of the application.
     */
    name: 'BooksApp',

    data() {
      return {
        books: [],
        book: {
          id: '',
          authors: [],
          title: '',
          description: '',
          publisher: '',
          thumbnail: '',
        },
        query: "football",
        chosenBook: '',
        showOneBook: false,
        showAllBooks: true
      };
    },

    /**
     * The methods of this Vue instance.
     */
    methods: {

      searchBooks() {

        this.books = [];
        this.fetchBooks(this.query);

      },

      fetchBooks(query) {
        this.axios.get("https://www.googleapis.com/books/v1/volumes?q=" + query)
          .then(({data}) => {

            const obj = data.items;

            for ( const key in obj ) {

              this.books.push(obj[key]);
            }

          });


      },

      showBook(book) {
        this.chosenBook = book;
        this.showAllBooks = false;
        this.showOneBook = true;
      },

      returnBack() {
        this.showAllBooks = true;
        this.showOneBook = false;
      }


    },
  }
</script>


<style>

body {
  background-color: #212121;
  text-align: center;
}


@import url(https://fonts.googleapis.com/css?family=Poppins:300,700);
.book-fig {
  font-family: 'Poppins:400,700', Arial, sans-serif;
  position: relative;
  display: inline-block;
  overflow: hidden;
  margin: 8px;
  min-width: 250px;
  max-width: 310px;
  width: 100%;
  background-color: #000000;
  color: #ffffff;
  text-align: left;
  font-size: 16px;
  box-shadow: 0 0 5px rgba(0, 0, 0, 0.15);
}
.book-fig * {
  -webkit-transition: all 0.35s;
  transition: all 0.35s;
  -webkit-box-sizing: border-box;
  box-sizing: border-box;
}
.book-fig img {
  max-width: 100%;
  vertical-align: top;
}
.book-fig figcaption {
  position: absolute;
  top: 0;
  bottom: 0;
  left: 0;
  right: 0;
  padding: 20px;
  background-image: -webkit-linear-gradient(bottom, rgba(0, 0, 0, 0.8) 0%, transparent 100%);
  background-image: linear-gradient(to top, rgba(0, 0, 0, 0.8) 0%, transparent 100%);
  display: flex;
  flex-direction: column;
  justify-content: flex-end;
}

.book-fig h2 {
  font-size: 40px;
  font-weight: 400;
  line-height: 1;
  letter-spacing: 1px;
  text-transform: uppercase;
  margin: 3px 0;
}

.book-fig h4 {
  font-size: 20px;
  font-weight: 400;
  line-height: 1;
  letter-spacing: 1px;
  text-transform: uppercase;
  margin: 3px 0;
}
.book-fig .title1 {
  font-weight: 700;
}
.book-fig .title2 {
  color: #a58e7c;
  font-weight: 300;
}
.book-fig .title3 {
  font-weight: 700;
  font-size: 15px;
}

#title1 {
  color: #a58e7c;
  font-weight: 700;
}
#title2 {
  color: #a58e7c;
  font-weight: 300;
}
#title3 {
  font-weight: 700;
  color: #a58e7c;
  font-size: 20px;
}

#paragraph {
  color: #a58e7c;
  font-size: 15px;
  font-weight: 200;
  text-align: left;
}

.book-fig a {
  position: absolute;
  top: 0;
  bottom: 0;
  left: 0;
  right: 0;
}
.book-fig:hover img,
.book-fig.hover img {
  -webkit-transform: scale(1.3) rotate(5deg);
  transform: scale(1.3) rotate(5deg);
}


@import url(https://fonts.googleapis.com/css?family=Roboto);
.button-style {
  font-family: 'Roboto', Arial, sans-serif;
  color: #ffffff;
  cursor: pointer;
  padding: 0px 40px;
  display: inline-block;
  margin: 15px 30px;
  text-transform: uppercase;
  line-height: 2.7em;
  letter-spacing: 1.5px;
  font-size: 1em;
  outline: none;
  position: relative;
  font-size: 16px;
  border: 3px solid #fff;
  background-color: transparent;
  border-radius: 15px 0 15px 15px;
  -webkit-transition: all 0.3s;
  transition: all 0.3s;
}

.button-style:before {
  content: "";
  position: absolute;
  right: -3px;
  top: -3px;
  width: 0;
  height: 0;
  border-style: solid;
  border-width: 0 35px 35px 0;
  border-color: transparent #c87f0a transparent transparent;
  z-index: 1;
}

.button-style:hover,
.button-style.hover {
  border-color: #c87f0a;
}


</style>
