<template>
  <!--
  <Saisirdate label="Du" v-model="jour"></Saisirdate>
  -->
  <v-text-field :label="label" v-bind:value="datefr" v-on:input="onDate($event)" :error="erreur">
    <template v-slot:append>
      <v-menu
        v-model="menu"
        :close-on-content-click="false"
        :nudge-right="40"
        transition="scale-transition"
        offset-y
        :max-width="width"
        :min-width="width"
      >
        <template v-slot:activator="{ on }">
          <v-btn icon v-on="on"
            ><v-icon>{{ icon }}</v-icon></v-btn
          >
        </template>
        <v-date-picker v-model="sdate" no-title locale="fr" first-day-of-week="1" @input="onPicker"> </v-date-picker>
      </v-menu>
    </template>
  </v-text-field>
</template>
<script>
export default {
  data() {
    return {
      menu: null,
      sdate: null,
      datefr: null,
      erreur: false
    }
  },
  created() {
    this.sdate = this.value
    const [year, month, day] = this.value.split('-')
    this.datefr = `${day}/${month}/${year}`
  },
  watch: {
    value() {
      this.sdate = this.value
      this.toFrench()
    }
  },
  props: {
    value: {
      type: String,
      default: null
    },
    label: {
      type: String,
      default: ''
    },
    width: {
      type: Number,
      default: 290
    },
    icon: {
      type: String,
      default: 'event'
    }
  },
  methods: {
    toFrench() {
      const [year, month, day] = this.sdate.split('-')
      this.datefr = `${day}/${month}/${year}`
    },
    onDate(dte) {
      const [day, month, year] = dte.split('/')
      var s = `${year}-${month}-${day}`
      var d = Date.parse(s)
      if (!isNaN(d)) {
        this.sdate = s
        this.datefr = dte
        this.$emit('input', this.sdate)
        this.erreur = false
      } else {
        this.erreur = true
      }
    },
    onPicker() {
      this.menu = false
      this.toFrench()
      this.$emit('input', this.sdate)
    }
  }
}
</script>
