<template>
  <div>
    <div class="ui vertical segment">
      <div class="flexbox">
        <div class="flex-content">
          <h3>Dynamic Search with ajax (country name)</h3>
          <div>
            <model-list-select
              :list="countries"
              option-value="code"
              option-text="name"
              v-model="selectedCountry"
              placeholder="select item"
              @searchchange="searchCountry"
            >
            </model-list-select>
          </div>
        </div>
        <div class="flex-result">
          <h4>input text(searchText)</h4>
          <p>{{ searchText }}</p>
          <table class="ui celled table">
            <thead>
            <tr>
              <th>code</th>
              <th>name</th>
            </tr>
            </thead>
            <tbody>
            <tr>
              <td>{{ selectedCountry.code }}</td>
              <td>{{ selectedCountry.name }}</td>
            </tr>
            </tbody>
          </table>
        </div>
      </div>
    </div>
    <div class="ui vertical segment">
      <div class="flexbox">
        <div class="flex-content">
          <h3>Dynamic Search with ajax and 500ms debounce (country name)</h3>
          <div>
            <model-list-select
              :list="countries"
              option-value="code"
              option-text="name"
              v-model="selectedCountryDebounce"
              debounce="500"
              placeholder="select item"
              @searchchange="searchCountry"
            >
            </model-list-select>
          </div>
        </div>
        <div class="flex-result">
          <h4>input text(searchText)</h4>
          <p>{{ searchText }}</p>
          <table class="ui celled table">
            <thead>
            <tr>
              <th>code</th>
              <th>name</th>
            </tr>
            </thead>
            <tbody>
            <tr>
              <td>{{ selectedCountryDebounce.code }}</td>
              <td>{{ selectedCountryDebounce.name }}</td>
            </tr>
            </tbody>
          </table>
        </div>
      </div>
    </div>
    <div class="ui vertical segment">
      <div class="flexbox">
        <div class="flex-content">
          <h3>Init with ajax</h3>
          <div>
            <model-list-select
              :list="animations"
              option-value="id"
              :custom-text="optionDisplayText"
              v-model="selectedAnimation"
              placeholder="select item"
              @searchchange="printSearchText"
            >
            </model-list-select>
          </div>
        </div>
        <div class="flex-result">
          <h4>input text(searchText)</h4>
          <p>{{ searchText2 }}</p>
          <table class="ui celled table">
            <thead>
            <tr>
              <th>id</th>
              <th>title_short1</th>
              <th>twitter_account</th>
              <th>public_url</th>
            </tr>
            </thead>
            <tbody>
            <tr>
              <td>{{ selectedAnimation.id }}</td>
              <td>{{ selectedAnimation.title_short1 }}</td>
              <td>{{ selectedAnimation.twitter_account }}</td>
              <td>{{ selectedAnimation.public_url }}</td>
            </tr>
            </tbody>
          </table>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import { ModelListSelect } from "vue-search-select"
import axios from "axios"
import { ajaxFindCountry } from "../data/countriesApi"

export default {
  data() {
    return {
      countries: [],
      selectedCountry: {},
      selectedCountryDebounce: {},
      searchText: "",
      searchTextDebounce: "",
      animations: [],
      selectedAnimation: {},
      searchText2: "",
    }
  },
  created() {
    this.searchAnimation()
  },
  methods: {
    searchCountry(searchText) {
      this.searchText = searchText
      ajaxFindCountry(searchText).then(response => {
        this.countries = response
      })
    },
    searchCountryDebounce(searchText) {
      this.searchTextDebounce = searchText
      ajaxFindCountry(searchText).then(response => {
        this.countries = response
      })
    },
    printSearchText(searchText) {
      this.searchText2 = searchText
    },
    /**
     * api : https://qiita.com/AKB428/items/64938febfd4dcf6ea698
     */
    searchAnimation() {
      if (this.animations.length === 0) {
        axios.get(`http://api.moemoe.tokyo/anime/v1/master/2017/2`)
          .then(response => {
            this.animations = response.data
          })
          .catch(error => {
            /* eslint no-console: off */
            console.error(error)
          })
      }
    },
    optionDisplayText(animation) {
      return `${animation.title_short1} - ${animation.twitter_account} - ${animation.public_url}`
    },
  },
  components: {
    ModelListSelect,
  },
}
</script>
