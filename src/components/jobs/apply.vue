<template>
  <div class="file-upload">
      <h1>Apply for job</h1>
      <va-notification v-if="errorMessageFromServer" color="danger" closeable>
          <va-badge color="danger">
            Error
          </va-badge>
          {{ errorMessageFromServer }}
        </va-notification>
        <va-notification v-if="successMessage" color="success" closeable>
          <va-badge color="success">
            Success
          </va-badge>
          {{ successMessage }}
        </va-notification>
    <div class="row">
      <div class="flex xs12">
        <va-card title="CV">
          <va-file-upload
            type="single"
            file-types=".pdf"
            v-model="cv"
          />
        </va-card>
      </div>
      <div class="flex xs12">
        <va-card title="Cover Letter">
          <va-file-upload
            type="single"
            file-types=".pdf"
            v-model="coverletter"
          />
        </va-card>
      </div>
      <div class="flex xs12">
        <va-input
            label="Description"
            v-model="description"
            placeholder="Say something ...."
        />
      </div>
      <va-inner-loading class="flex-center py-3" :loading="isLoading">
        <va-button @click="submitApplication()">
            Apply
        </va-button>
      </va-inner-loading>
    </div>
  </div>
</template>

<script>
import { callApi } from '@/app/utils'
export default {
  name: 'apply',
  data () {
    return {
      errorMessageFromServer: null,
      successMessage: null,
      isLoading: false,
      cv: '',
      coverletter: '',
      description: '',
      jobuuid: '',
    }
  },
  created () {
    if (!this.$route.params.uuid) {
      this.$router.push('/joblist')
    } else {
      this.jobuuid = this.$route.params.uuid
    }
  },
  methods: {
    submitApplication () {
      let self = this
      this.isLoading = true
      let formData = new FormData()
      formData.append('cv', self.cv[0])
      formData.append('cover', self.coverletter[0])
      formData.append('description', self.description)

      callApi(`/jobs/apply/${self.jobuuid}`, formData, 'post')
        .then(res => {
          this.errorMessageFromServer = null
          this.successMessage = res.message
          this.cv = ''
          this.coverletter = ''
        })
        .catch(err => {
          this.successMessage = null
          this.errorMessageFromServer = err.message
        })
    },
  },
}
</script>

<style lang="scss">
.cards-container {
  .va-card {
    margin: 0;
  }
}
</style>
