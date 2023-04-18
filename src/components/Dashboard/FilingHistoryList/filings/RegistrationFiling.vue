<template>
  <FilingTemplate class="registration-filing" :filing="filing" :index="index">
    <template #body>
      <div v-if="isStatusCompleted" class="completed-dissolution-details">
        <h4>Registration Complete</h4>

        <p>{{ getLegalName || 'This company' }} has been successfully registered.</p>

        <p>Return to My Business Registry to access your business and file changes.</p>

        <div class="to-dashboard-container text-center mt-6">
          <v-btn color="primary" @click.stop="returnToMyBusinessRegistry()">
            <span>Return to My Business Registry</span>
          </v-btn>
        </div>
      </div>
    </template>
  </FilingTemplate>
</template>

<script lang="ts">
import Vue from 'vue'
import { Component, Prop } from 'vue-property-decorator'
import { Getter } from 'vuex-class'
import { ApiFilingIF } from '@/interfaces'
import { EnumUtilities } from '@/services'
import { navigate } from '@/utils'
import FilingTemplate from '../FilingTemplate.vue'

@Component({
  components: {
    FilingTemplate
  }
})
export default class RegistrationFiling extends Vue {
  @Prop({ required: true }) readonly filing!: ApiFilingIF
  @Prop({ required: true }) readonly index!: number

  @Getter readonly getLegalName!: string
  @Getter readonly getMyBusinessRegistryUrl!: string

  get isStatusCompleted (): boolean {
    return EnumUtilities.isStatusCompleted(this.filing)
  }

  protected returnToMyBusinessRegistry (): void {
    navigate(this.getMyBusinessRegistryUrl)
  }
}
</script>

<style lang="scss" scoped>
@import "@/assets/styles/theme.scss";

p {
  color: $gray7;
  font-size: $px-15;
  margin-top: 1rem !important;
}
</style>