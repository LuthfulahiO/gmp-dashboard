<template>
  <div class="form-elements">
    <div class="row">
      <div class="flex xs12">
        <h1 class="headerOne">Update Education</h1>
          <form @submit.prevent="updateProfile">
            <va-card title="Education">
            <div class="row" v-for="education in profile.education" :key="education.institute">
              <div class="flex md4 sm6 xs12">
                <va-input
                  label="Institute"
                  v-model="education.institute"
                  placeholder="Institute"
                />
              </div>
              <div class="flex md4 sm6 xs12">
                <va-input
                  label="Qualification"
                  v-model="education.qualification"
                  placeholder="SSCE"
                />
              </div>
              <div class="flex md4 sm6 xs12">
                <va-input
                  label="Field Of Study"
                  v-model="education.field_of_study"
                  placeholder="Yoruba Tech"
                />
              </div>
              <div class="flex md4 sm6 xs12">
                <va-input
                  label="Year Of Graduation"
                  v-model="education.year_of_graduation"
                  placeholder="2002"
                />
              </div>
            </div>
            <div class="flex md4 sm6 xs12">
                <va-button v-if="!isSubmit" type="submit" class="my-0">Update</va-button>
                <fingerprint-spinner
                v-if="isSubmit"
                :animation-duration="1500"
                :size="64"
                :color="'#4ae387'"
                />
              </div>
            </va-card>
          </form>
          <div class="flex md4 sm6 xs12">
                <va-button @click="addNewEducation" icon="ion-md-add ion" class="my-0">Add education</va-button>
           </div>
        <!-- </va-card> -->
      </div>
    </div>
  </div>
</template>

<script>
import { callApi } from '@/app/utils'
import { FingerprintSpinner } from 'epic-spinners'
export default {
  name: 'Education',
  components: { FingerprintSpinner },
  data () {
    return {
      isSubmit: false,
      isLoading: false,
      gallery: [],
      profile: {
        'grade': '',
        'title': '',
        'marital_status': '',
        'dob': '',
        'gender': '',
        'phone_number': '',
        'address': '',
        'city': '',
        'state': '',
        'country': '',
        'postal_address': '',
        'chapter': '',
        'sector': '',
        'sub_sector': '',
        'occupation': '',
        'education': [
          {
            'institute': '',
            'qualification': '',
            'field_of_study': '',
            'year_of_graduation': '',
          },
        ],
        'position_rank': '',
        'company': '',
        'location': '',
        'work_role': '',
        'start_date': '',
        'end_date': '',
        'description': '',
      },
    }
  },
  methods: {
    updateProfile () {
      this.isSubmit = true
      callApi('/profile/update/education', this.profile.education, 'post')
        .then(res => {
          this.isSubmit = false
          console.log(res)
        })
        .catch(err => {
          this.isSubmit = false
          console.log(err)
        })
    },
    addNewEducation () {
      this.profile.education.push({
        'institute': '',
        'qualification': '',
        'field_of_study': '',
        'year_of_graduation': '',
      })
    },
  },
  created () {
    callApi(`/profile`, {}, 'get')
      .then(res => {
        console.log(res)
        this.profile = res.data
      })
      .catch(err => {
        console.log(err)
      })
  },
}
</script>

<style>
  .row.row-inside {
    max-width: none;
  }
  .headerOne {
    margin: 0px 0px 20px 10px;
  }
</style>
