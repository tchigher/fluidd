<template>
  <div>
  <v-row justify="end">
    <v-col cols="6" class="text-right">
      <v-text-field
        v-model="extrudeLength"
        :disabled="!klippyConnected"
        @focus="$event.target.select()"
        outlined
        dense
        hide-details
        single-line
        label="Extrude Length"
        type="number"
        suffix="mm">
      </v-text-field>
    </v-col>
    <v-col cols="6">
      <v-btn
        @click="sendRetractGcode(extrudeLength, extrudeSpeed, waits.onExtract)"
        :disabled="hasWaits || !extrudeRetractReady || !klippyConnected"
        :elevation="2"
        block
        color="secondary"
        class="mr-2">
        Retract
        <v-icon>$chevronUp</v-icon>
      </v-btn>
    </v-col>
  </v-row>
  <v-row justify="end">
    <v-col cols="6" class="text-right">
      <v-text-field
        v-model="extrudeSpeed"
        :disabled="!klippyConnected"
        @focus="$event.target.select()"
        outlined
        dense
        hide-details
        single-line
        label="Extrude Speed"
        suffix="mm/s">
      </v-text-field>
    </v-col>
    <v-col cols="6">
      <v-btn
        @click="sendExtrudeGcode(extrudeLength, extrudeSpeed, waits.onExtrude)"
        :disabled="hasWaits || !extrudeRetractReady || !klippyConnected"
        :elevation="2"
        block
        color="secondary"
        class="mr-2">
        Extrude
        <v-icon>$chevronDown</v-icon>
      </v-btn>
    </v-col>
  </v-row>
  </div>
</template>

<script lang="ts">
import { Component, Mixins } from 'vue-property-decorator'
import BtnToolheadMove from '@/components/inputs/BtnToolheadMove.vue'
import UtilsMixin from '@/mixins/utils'
import { Waits } from '@/globals'

@Component({
  components: {
    BtnToolheadMove
  }
})
export default class ToolheadMovesWidget extends Mixins(UtilsMixin) {
  waits = Waits
  feedSpeed = -1
  feedLength = -1

  get extrudeSpeed () {
    return (this.feedSpeed === -1)
      ? this.$store.state.config.fileConfig.general.defaultExtrudeSpeed
      : this.feedSpeed
  }

  set extrudeSpeed (val: number) {
    this.feedSpeed = val
  }

  get extrudeLength () {
    return (this.feedLength === -1)
      ? this.$store.state.config.fileConfig.general.defaultExtrudeLength
      : this.feedLength
  }

  set extrudeLength (val: number) {
    this.feedLength = val
  }
}
</script>
