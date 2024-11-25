<template>
    <v-container fluid>
      <v-card class="mx-auto pa-4 mt-4" max-width="1200">
        <!-- Header -->
        <div class="d-flex flex-wrap align-center mb-6">
          <v-img src="@/assets/safaricom-logo.png" max-width="150" class="mr-4 mb-2"></v-img>
          <h1 class="text-h4 green--text text--darken-2">OLT DATA FORM</h1>
        </div>
  
        <!-- Alert Snackbar -->
        <v-snackbar
          v-model="snackbar.show"
          :color="snackbar.color"
          :timeout="3000"
          top
        >
          {{ snackbar.text }}
          <template v-slot:action="{ attrs }">
            <v-btn
              text
              v-bind="attrs"
              @click="snackbar.show = false"
            >
              Close
            </v-btn>
          </template>
        </v-snackbar>
  
        <!-- Form -->
        <v-form @submit.prevent="submitForm" ref="form" v-model="valid">
          <v-tabs v-model="tab" background-color="#51b44b" dark>
            <v-tab>Basic Info</v-tab>
            <v-tab>Location & Agency</v-tab>
            <v-tab>Additional Details</v-tab>
          </v-tabs>
  
          <v-tabs-items v-model="tab">
            <!-- Basic Info Tab -->
            <v-tab-item>
              <v-container>
                <v-row>
                  <v-col cols="12" md="6">
                    <v-text-field
                      v-model="form.OLT_ID"
                      label="OLT ID*"
                      type="number"
                      :rules="[v => !!v || 'OLT ID is required']"
                      outlined
                      dense
                    ></v-text-field>
                  </v-col>
                  
                  <v-col cols="12" md="6">
                    <v-select
                      v-model="form.STATUS"
                      :items="['active', 'inactive']"
                      label="Status*"
                      outlined
                      dense
                      required
                    ></v-select>
                  </v-col>
  
                  <v-col cols="12" md="6">
                    <v-text-field
                      v-model="form.DEV_IP"
                      label="Device IP"
                      outlined
                      dense
                      :rules="[v => !!v || 'OLT ID is required', v => !v || v.length <= 30 || 'Max 30 characters']"
                    ></v-text-field>
                  </v-col>
  
                  <v-col cols="12" md="6">
                    <v-text-field
                      v-model="form.DEVNAME"
                      label="Device Name"
                      outlined
                      dense
                      :rules="[v => !v || v.length <= 70 || 'Max 70 characters']"
                    ></v-text-field>
                  </v-col>
  
                  <v-col cols="12" md="6">
                    <v-text-field
                      v-model="form.MODEL"
                      label="Model"
                      outlined
                      dense
                      :rules="[v => !v || v.length <= 40 || 'Max 40 characters']"
                    ></v-text-field>
                  </v-col>
  
                  <v-col cols="12" md="6">
                    <v-menu
                      ref="menu"
                      v-model="menu"
                      :close-on-content-click="false"
                      transition="scale-transition"
                    >
                      <template v-slot:activator="{ on, attrs }">
                        <v-text-field
                          v-model="form.INSERT_DATE"
                          label="Insert Date"
                          readonly
                          v-bind="attrs"
                          v-on="on"
                          outlined
                          dense
                        ></v-text-field>
                      </template>
                      <v-date-picker
                        v-model="form.INSERT_DATE"
                        @input="menu = false"
                      ></v-date-picker>
                    </v-menu>
                  </v-col>
                </v-row>
              </v-container>
            </v-tab-item>
  
            <!-- Location & Agency Tab -->
            <v-tab-item>
              <v-container>
                <v-row>
                  <v-col cols="12" md="6">
                    <v-text-field
                      v-model="form.LONGITUDE"
                      label="Longitude"
                      type="number"
                      step="any"
                      outlined
                      dense
                    ></v-text-field>
                  </v-col>
  
                  <v-col cols="12" md="6">
                    <v-text-field
                      v-model="form.LATITUDE"
                      label="Latitude"
                      type="number"
                      step="any"
                      outlined
                      dense
                    ></v-text-field>
                  </v-col>
  
                  <v-col cols="12" md="6">
                    <v-text-field
                      v-model="form.MAINTENANCE_AGENCY_NAME"
                      label="Maintenance Agency Name"
                      outlined
                      dense
                      :rules="[v => !v || v.length <= 255 || 'Max 255 characters']"
                    ></v-text-field>
                  </v-col>
  
                  <v-col cols="12" md="6">
                    <v-text-field
                      v-model="form.ROLLOUT_AGENCY_NAME"
                      label="Rollout Agency Name"
                      outlined
                      dense
                      :rules="[v => !v || v.length <= 255 || 'Max 255 characters']"
                    ></v-text-field>
                  </v-col>
  
                  <v-col cols="12" md="6">
                    <v-text-field
                      v-model="form.MAINTENANCE_AGENCY_ID"
                      label="Maintenance Agency ID"
                      type="number"
                      outlined
                      dense
                    ></v-text-field>
                  </v-col>
  
                  <v-col cols="12" md="6">
                    <v-text-field
                      v-model="form.ROLLOUT_AGENCY_ID"
                      label="Rollout Agency ID"
                      type="number"
                      outlined
                      dense
                    ></v-text-field>
                  </v-col>
                </v-row>
              </v-container>
            </v-tab-item>
  
            <!-- Additional Details Tab -->
            <v-tab-item>
              <v-container>
                <v-row>
                  <v-col cols="12" md="6">
                    <v-text-field
                      v-model="form.CONTRACTOR_AGENCY_NAME"
                      label="Contractor Agency Name"
                      outlined
                      dense
                      :rules="[v => !v || v.length <= 70 || 'Max 70 characters']"
                    ></v-text-field>
                  </v-col>
  
                  <v-col cols="12" md="6">
                    <v-text-field
                      v-model="form.DEV_VLAN_ID"
                      label="VLAN ID"
                      outlined
                      dense
                      :rules="[v => !v || v.length <= 50 || 'Max 50 characters']"
                    ></v-text-field>
                  </v-col>
  
                  <v-col cols="12" md="6">
                    <v-text-field
                      v-model="form.VENDOR_ID"
                      label="Vendor ID"
                      type="number"
                      outlined
                      dense
                    ></v-text-field>
                  </v-col>
  
                  <v-col cols="12" md="6">
                    <v-text-field
                      v-model="form.VENDOR_NAME"
                      label="Vendor Name"
                      outlined
                      dense
                      :rules="[v => !v || v.length <= 255 || 'Max 255 characters']"
                    ></v-text-field>
                  </v-col>
  
                  <v-col cols="12" md="6">
                    <v-text-field
                      v-model="form.LOS_TICKETS"
                      label="LOS Tickets*"
                      type="number"
                      :rules="[v => v >= 0 && v <= 1 || 'Value must be 0 or 1']"
                      outlined
                      dense
                      required
                    ></v-text-field>
                  </v-col>
  
                  <v-col cols="12" md="6">
                    <v-text-field
                      v-model="form.LOS_NOTIFICATIONS"
                      label="LOS Notifications*"
                      type="number"
                      :rules="[v => v >= 0 && v <= 1 || 'Value must be 0 or 1']"
                      outlined
                      dense
                      required
                    ></v-text-field>
                  </v-col>
                </v-row>
              </v-container>
            </v-tab-item>
          </v-tabs-items>
  
          <!-- Submit Button -->
          <v-card-actions>
            <v-btn
              type="submit"
              block
              x-large
              :color="'#51b44b'"
              class="mt-6 white--text"
              :disabled="!valid"
            >
              Submit
            </v-btn>
          </v-card-actions>
        </v-form>
      </v-card>
    </v-container>
  </template>
  
  <script>
  import axios from '@/plugins/axios'
  
  export default {
    name: 'OltForm',
    
    data: () => ({
      valid: false,
      tab: 0,
      menu: false,
      snackbar: {
        show: false,
        text: '',
        color: ''
      },
      form: {
        OLT_ID: null,
        DEV_IP: '',
        DEVNAME: '',
        MODEL: '',
        INSERT_DATE: null,
        CONTRACTOR_AGENCY_NAME: '',
        DEV_VLAN_ID: '',
        VENDOR_ID: null,
        LOS_TICKETS: 0,
        LOS_NOTIFICATIONS: 0,
        ROLLOUT_AGENCY_ID: null,
        MAINTENANCE_AGENCY_ID: null,
        LONGITUDE: null,
        LATITUDE: null,
        STATUS: 'active',
        MAINTENANCE_AGENCY_NAME: '',
        ROLLOUT_AGENCY_NAME: '',
        VENDOR_NAME: ''
      }
    }),
  
    methods: {
      async submitForm() {
        if (this.$refs.form.validate()) {
          try {
            await axios.post('/api/olt', this.form)
            this.$refs.form.reset()
            this.showSuccessAlert()
          } catch (error) {
            this.showErrorAlert(error)
          }
        }
      },
  
      showSuccessAlert() {
        this.snackbar.color = 'success'
        this.snackbar.text = 'OLT data saved successfully!'
        this.snackbar.show = true
      },
  
      showErrorAlert(error) {
        this.snackbar.color = 'error'
        this.snackbar.text = `Error: ${error.message}`
        this.snackbar.show = true
      }
    }
  }
  </script>
  
  <style scoped>
  .v-text-field.v-text-field--enclosed .v-text-field__details {
    margin-bottom: 0;
  }
  </style>