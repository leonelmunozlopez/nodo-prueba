<template>
  <div>
    <h1 v-show="isLoading">Cargando personajes ...</h1>
    <table v-show="!isLoading">
      <tr>
        <th>Nombre</th>
        <th>Casa</th>
        <th>Detalle</th>
      </tr>
      <tr v-for="character in characters">
        <td>{{ character.name }}</td>
        <td>{{ character.house }}</td>
        <td>
          <button @click="goToDetail(character._id)">Ver detalle</button>
        </td>
      </tr>
    </table>

    <!-- MODAL -->
    <span v-show="isLoadingDetails" class="loading-details"></span>
    <div v-if="detail.name" class="modal-container">
      <div class="modal-body">
        <span @click="closeModal()" class="modal-close">&times;</span>

        <div class="modal-content">
          <h2>{{ detail.name }}</h2>
          <h4>{{ detail.house }}</h4>

          <div v-if="detail.imageLink" class="image">
            <img :src="detail.imageLink">
          </div>

          <dl>
            <dt v-if="detail.createdAt">Created at</dt>
            <dd v-if="detail.createdAt">{{ detail.createdAt }}</dd>

            <dt v-if="detail.dateOfBirth">Date of birth</dt>
            <dd v-if="detail.dateOfBirth">{{ detail.dateOfBirth }}</dd>

            <dt v-if="detail.DateOfDeath">Date of death at</dt>
            <dd v-if="detail.DateOfDeath">{{ detail.DateOfDeath }}</dd>

            <dt v-if="detail.father">Father</dt>
            <dd v-if="detail.father">{{ detail.father }}</dd>

            <dt v-if="detail.mother">Mother</dt>
            <dd v-if="detail.mother">{{ detail.mother }}</dd>

            <dt v-if="detail.heir">Heir</dt>
            <dd v-if="detail.heir">{{ detail.heir }}</dd>

            <dt v-if="detail.culture">Culture</dt>
            <dd v-if="detail.culture">{{ detail.culture }}</dd>
          </dl>
        </div>
      </div>
    </div>
  </div>
</template>




<script>
import { listsAllCharacters, getACharacter } from "../services/got.service.js";

export default {
  name: "list-component",

  /**
   * @description the data block represents all the local variable of this component.
   */
  data() {
    return {
      characters: [],
      detail: {},
      isLoading: false,
      isLoadingDetails: false
    };
  },

  /**
   * @description the create function is the first one to be execute when the component is being created (see vue js lifecycle).
   */
  created() {
    this.isLoading = true;
    listsAllCharacters().then(res => {
      this.characters = res;
      this.isLoading = false;
    });
  },

  /**
   * @description the methods block represents all the local methods of this component.
   */
  methods: {
    /**
     * @description get the detail of a character from the GoT API.
     * @param {string} id. the "_id" of the character that we are going to request.
     * @method goToDetail
     */
    goToDetail(id) {
      this.isLoadingDetails = true;
      getACharacter(id).then(response => {
        this.detail = response.data;
        this.detail.imageLink = "https://api.got.show" + this.detail.imageLink;
        this.isLoadingDetails = false;
      });
    },

    closeModal() {
      this.detail = {};
    }
  }
};
</script>

<style>
table {
  font-family: arial, sans-serif;
  border-collapse: collapse;
  width: 100%;
}

td,
th {
  border: 1px solid #dddddd;
  text-align: left;
  padding: 8px;
}

tr:nth-child(even) {
  background-color: #dddddd;
}

.loading-details {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: url(https://www.honeypot2night.com/wp-content/uploads/2018/04/ajax-loading-gif.gif)
    center center no-repeat;
}

.modal-container {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  width: auto;
  height: auto;
  background: rgba(255, 255, 255, 0.6);
  z-index: 999;
}

.modal-body {
  position: relative;
  margin: 25px auto;
  width: 80%;
  background: #fff;
  border: 1px solid #ccc;
}

.modal-content {
  padding: 15px;
}

.modal-content h2 {
  color: #222;
  border-bottom: 1px solid #333;
}

.modal-content:before,
.modal-content:after {
  content: " ";
  display: table;
}
.modal-content:after {
  clear: both;
}

.modal-close {
  position: absolute;
  top: 15px;
  right: 15px;
  padding: 8px 12px;
  background: #333;
  color: #fff;
  text-align: center;
  cursor: pointer;
}

.image {
  float: right;
  width: 320px;
}

.image img {
  display: block;
  width: 100%;
  height: auto;
}

dt {
  font-weight: bold;
  color: #444;
}

dd {
  margin-bottom: 12px;
}
</style>