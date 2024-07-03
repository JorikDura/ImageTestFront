<script>
import axios from "axios";
import CardImage from "@/components/CardImage.vue";

export default {
  name: "MainContent",
  components: {CardImage},
  data() {
    return {
      images: null,
      pages: null,
      links: null,
      currentPage: '1',
      currentSortType: 'created_at'
    }
  },
  mounted() {
    this.getImages()
  },
  methods: {
    getImages() {
      axios.get("http://localhost/api/v1/image", {
        params: {
          page: this.currentPage,
          order_by: this.currentSortType
        }
      }).then((response) => {
        this.images = response.data.data;
        this.pages = response.data.meta.last_page;
        this.links = response.data.links;
      })
    },
    sendImages(e) {
      let formData = new FormData();
      let files = e.target.files;
      for (let i = 0; i < files.length; i++) {
        formData.append("images[]", files[i]);
      }

      axios.post("http://localhost/api/v1/image", formData, {
        headers: {
          "Content-Type": "multipart/form-data"
        },
      }).then((response) => {
        if (response.status === 200) {
          this.getImages()
        }
      }).catch((error) => {
        console.log(error);
      })
    },
    goToPage(e) {
      if (this.currentPage === e.target.innerText) {
        return;
      }

      this.currentPage = e.target.innerText;
      this.getImages();
    },
    changeSorting(e) {
      if (this.currentSortType === e.target.id) {
        return;
      }

      this.currentSortType = e.target.id;
      this.getImages();
    }
  },
}
</script>

<template>
  <div class="buttons">
    <label for="images-upload" class="button">
      <input id="images-upload" type="file" multiple name="images" @change="sendImages"
             accept="image/png, image/jpg, image/jpeg"/>
      Загрузить картинки
    </label>

    <div class="dropdown">
      <button class="button">Сортировка</button>
      <div class="dropdown-content">
        <div @click="changeSorting" id="name">По названию</div>
        <div @click="changeSorting" id="created_at">По дате</div>
      </div>
    </div>
  </div>

  <div class="card-list">
    <div v-for="image in images" :key="image.id">
      <CardImage :image="image"/>
    </div>
  </div>

  <div class="pagination">
    <div v-for="index in pages" :key="index">
      <div class="pagination__item" @click="goToPage">{{ index }}</div>
    </div>
  </div>

</template>

<style scoped lang="scss">
.buttons {
  display: flex;
  justify-content: space-between;
  margin: 75px 40px 40px 40px
}

input[type=file] {
  display: none;
}

.button {
  border: none;
  border-radius: 8px;
  background: linear-gradient(to right, #69DB8B, #53D8B9);
  padding: 24px 24px;
  cursor: pointer;
  font-size: 16px;
}

.dropdown {
  position: relative;
}

.dropdown-content {
  display: none;
  position: absolute;
  background-color: #f9f9f9;
  min-width: 160px;
  z-index: 1;
}

.dropdown-content div {
  color: black;
  padding: 12px 16px;
  text-decoration: none;
  display: block;
  cursor: pointer;
}

.dropdown-content div:hover {
  background-color: #f1f1f1
}

.dropdown:hover .dropdown-content {
  display: block;
}

.card-list {
  display: flex;
  margin-bottom: 35px;
  justify-content: center;
  flex-wrap: wrap;
  gap: 20px;
}

.pagination {
  display: flex;
  justify-content: center;
  margin-bottom: 35px;
  gap: 10px;
}

.pagination__item {
  cursor: pointer;
  padding: 5px 12px;
  border-radius: 8px;
  background-color: #69DB8B;
  transition: background-color 0.3s;
}

.pagination__item:hover {
  background-color: #53D8B9;
}
</style>