<template>
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
      <div class="q-pa-md">
        <q-table
          title="Audio Control"
          :data="results"
          :columns="columns"
          row-key="name"
          dark
          color="amber"
          :loading="loading"
        >
          <q-tr slot="body" slot-scope="props" :props="props" @click.native="$router.push({name:'call',params:{id:props.row.__index}})">
            <q-td key="calldate" :props="props">
              {{ props.row.calldate }}
            </q-td>
            <q-td key="dst" :props="props">
              {{ props.row.dst }}
            </q-td>
            <q-td key="src" :props="props">
              {{ props.row.src }}
            </q-td>
            <q-td key="billsec" :props="props">{{ props.row.billsec }}</q-td>
            <q-td key="check" :props="props"><q-checkbox v-model="props.row.check" disable /></q-td>
            <q-td key="score" :props="props">{{ props.row.score }}</q-td>
            <q-td key="userfield" :props="props"><audio controls><source :src="'https://api2.flipzip.ru/acr/audio/'+props.row.userfield"></audio></q-td>
          </q-tr>
          <q-td slot="body-cell-userfield" slot-scope="props" :props="props">
            <audio controls><source :src="'https://api2.flipzip.ru/acr/audio/'+props.value"></audio>
          </q-td>
        </q-table>
      </div>
    </q-page-container>
  </q-layout>
</template>

<script>
export default {
  name: 'MyLayout',

  data () {
    return {
      loading: false,
      results: [],
      columns: [
        {
          name: 'calldate',
          label: 'Время звонка',
          align: 'left',
          field: 'calldate',
          sortable: true
        },
        { name: 'dst', align: 'center', label: 'Номер абонента', field: 'dst', sortable: true },
        { name: 'src', label: 'Номер оператора', field: 'src', sortable: true },
        { name: 'billsec', label: 'Длительность звонка', field: 'billsec', sortable: true },
        { name: 'check', label: 'Проверен', field: 'check' },
        { name: 'score', label: 'Оценка', field: 'score', sortable: true },
        { name: 'userfield', label: 'Запись', field: 'userfield' }
      ]
    }
  },

  mounted () {
    this.loading = true
    this.$axios.get('https://api2.flipzip.ru/cdr').then(response => {
      this.results = response.data
      this.loading = false
    })
  }
}
</script>
