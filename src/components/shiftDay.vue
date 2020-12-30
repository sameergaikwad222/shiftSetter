<template>
  <v-container>
    <v-card
      @click="dialog = true"
      :color="isCompleted ? 'success' : 'error'"
      height="100"
      width="100"
      class="d-flex flex-column align-center justify-center text-center"
    >
      <v-card-text> Day </v-card-text>
      <v-card-text> {{ day }} </v-card-text>
    </v-card>
    <v-dialog v-model="dialog" persistent max-width="400">
      <v-card max-width="400" class="mx-auto">
        <v-card-title>{{ day }}</v-card-title>
        <v-list>
          <template v-for="(shift, i) in shifts">
            <v-list-item :key="i" three-line>
              <v-list-item-avatar>
                <v-img :src="shift.avatar"></v-img>
              </v-list-item-avatar>

              <v-list-item-content>
                <v-list-item-title
                  v-html="
                    shift.person === ''
                      ? personName(shift.title.toLowerCase())
                      : shift.person
                  "
                ></v-list-item-title>
                <v-list-item-subtitle
                  v-html="shift.title"
                ></v-list-item-subtitle>
                <v-list-item-subtitle
                  v-html="shift.details"
                ></v-list-item-subtitle>
              </v-list-item-content>
            </v-list-item>
          </template>
        </v-list>
        <v-card-actions>
          <v-btn outlined color="success" @click="dialog = false">
            Close
          </v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>
  </v-container>
</template>

<script>
import shifts from "@/data/shifts";
export default {
  props: {
    day: String,
    shiftPersons: Object
  },
  data() {
    return {
      shifts,
      dialog: false
    };
  },
  methods: {
    personName(shift) {
      return this.shiftPersons[shift].nick;
    }
  },
  computed: {
    isCompleted() {
      if (!this.shiftPersons.length === this.shifts.length) {
        return false;
      } else {
        return true;
      }
    }
  }
};
</script>

<style scoped></style>
