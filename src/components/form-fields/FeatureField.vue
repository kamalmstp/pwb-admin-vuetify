<template>
  <div class="mr-4">
    <div>
      <div class="text-xs-left">
        {{$t(fieldDetails.labelTextTKey) }}
      </div>
      <v-flex xs12>
        <v-switch v-bind:label="fieldLabel" @change="booleanFieldHandler" v-model="fieldValue"></v-switch>
      </v-flex>
    </div>
  </div>
</template>
<script>
// import SwitchField from '@/components/form-fields/SwitchField'
// import { required, minLength } from 'vuelidate/lib/validators'
// import _ from 'lodash'
export default {
  components: {},
  props: ["resourceModel", "fieldDetails", "cancelPendingChanges"],
  data() {
    return {
      fieldValue: false,
    }
  },
  watch: {
    'cancelPendingChanges' (newValue, oldValue) {
      if (oldValue === false) {
        // when cancelPendingChanges on parent changes from 
        // false to true
        // reset model to its original value
        this.fieldValue = this.fieldDetails.originalValue
      }
    },
    resourceModel: {
      handler(newValue, oldVal) {
        this.fieldDetails.originalValue = false
        if (newValue) {
          // Features are stored on the server as a list of field keys
          // The resourceModel represents this list
          // Any feature in that list should have a value of true 
          // here in this FeatureField
          // (ie - the property in question has that feature)
          if (newValue.includes(this.fieldDetails.fieldName)) {
            // this.fieldLabel = "Yes"
            this.fieldValue = true
            this.fieldDetails.originalValue = true
          }
        }
      },
      // deep: true,
      immediate: true,
    },
  },
  computed: {
    fieldLabel() {
      if (this.fieldValue) {
        return "Yes"
      } else {
        return "No"
      }
    }
  },
  //   currentField: {
  //     get() {
  //       this.fieldDetails.originalValue = false
  //       if (this.resourceModel) {
  //         // Features are stored on the server as a list of field keys
  //         // The resourceModel represents this list
  //         // Any feature in that list should have a value of true
  //         // (ie - the property in question has that feature)
  //         if (this.resourceModel.includes(this.fieldDetails.fieldName)) {
  //           this.fieldLabel = "Yes"
  //           this.fieldDetails.originalValue = true
  //         }
  //       }
  //       return this.fieldDetails.originalValue
  //     },
  //   }
  // },
  methods: {
    booleanFieldHandler(newValue) {
      this.fieldDetails.newValue = newValue
      this.$store.dispatch('updatePendingPropertyFeatureChanges', this.fieldDetails, newValue)
    }
  }

}

</script>
