<template>
  <div class="form-elements">
    <div class="row">
      <div class="flex xs12">
        <h1 class="headerOne">Update Education</h1>
          <form @submit.prevent="updateProfile">
            <va-card title="Experience">
            <div class="row" v-for="experience in profile.experience" :key="experience.company">
              <div class="flex md4 sm6 xs12">
                <va-input
                  label="Position Rank"
                  v-model="experience.position_rank"
                  placeholder="Senior"
                />
              </div>
              <div class="flex md4 sm6 xs12">
                <va-input
                  label="Company"
                  v-model="experience.company"
                  placeholder="Menaget"
                />
              </div>
              <div class="flex md4 sm6 xs12">
                <va-input
                  label="Location"
                  v-model="experience.location"
                  placeholder="Lekki"
                />
              </div>
              <div class="flex md4 sm6 xs12">
                <va-input
                  label="Work Role"
                  v-model="experience.work_role"
                  placeholder="Managing Director"
                />
              </div>
              <div class="flex md4 sm6 xs12">
                <va-input
                  label="Description"
                  v-model="experience.description"
                  placeholder="Explain what you like"
                />
              </div>
              <div class="flex md4 sm6 xs12">
                <va-date-picker
                    label="Start Date"
                    v-model="experience.start_date"
                />
              </div>
              <div class="flex md4 sm6 xs12">
                <va-date-picker
                    label="End Date"
                    v-model="experience.end_date"
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
                <va-button @click="addNewExperience" icon="ion-md-add ion" class="my-0">Add experience</va-button>
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
        'experience': [
          {
            'position_rank': '',
            'company': '',
            'location': '',
            'work_role': '',
            'start_date': '',
            'end_date': '',
            'description': '',
          },
        ],
      },
    }
  },
  methods: {
    updateProfile () {
      this.isSubmit = true
      callApi('/profile/update/experience', this.profile.experience, 'post')
        .then(res => {
          this.isSubmit = false
          console.log(res)
        })
        .catch(err => {
          this.isSubmit = false
          console.log(err)
        })
    },
    addNewExperience () {
      this.profile.experience.push({
        'position_rank': '',
        'company': '',
        'location': '',
        'work_role': '',
        'start_date': '',
        'end_date': '',
        'description': '',
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
