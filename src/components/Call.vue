<template>
    <div>
      <q-layout view="lHh Lpr lFf">
      <q-header elevated>
        <q-toolbar>
          <q-toolbar-title>
            Quasar App
          </q-toolbar-title>
          <div>Quasar v{{ $q.version }}</div>
        </q-toolbar>
      </q-header>
      <q-page-container>
        <q-item-label class="time">{{callInfo.calldate}} Продолжительность {{callInfo.billsec}}</q-item-label>
        <div>
            <q-card dark flat bordered class="bg-grey-9 my-card">
              <q-card-section>
                <p :key="score" v-for="score in scoreInfo">{{ score.item }}</p>
              </q-card-section>
            </q-card>
        </div>
      </q-page-container>
      </q-layout>
      </div>
</template>

<script>
export default {
  data () {
    return {
      id: this.$route.params.id,
      callInfo: [],
      scoreInfo: []
    }
  },
  mounted () {
    this.$axios.get('https://api2.flipzip.ru/cdr').then(response => {
      this.callInfo = response.data[this.id]
    })
    this.$axios.get('https://api2.flipzip.ru/score_item').then(response => {
      this.scoreInfo = response.data
    })
  }
}
</script>

<style lang="sass" scoped>
.my-card
  width: 100%
  max-width: 250px
  margin-left: 25px
.time
  padding: 25px
</style>
