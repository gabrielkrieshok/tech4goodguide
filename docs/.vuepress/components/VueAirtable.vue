<template>

<div>
  <ul v-for="record in records" :key="record.id">
      <li style="list-style-type: none;">
        <a :href="record.fields['Link']" target="_blank"><img :src="record.fields['Logo'][0].thumbnails.small.url"/></a>
        <a :href="record.fields['Link']" target="_blank">{{ record.fields['Name'] }}</a> — {{ record.fields['Description'] }} (<em>Updated: {{ record.fields['Updated'] }}</em>)</br />
      </li>
  </ul>
</div>

</template>

<script>
import axios from 'axios';

export default {
  name: 'VueAirtable',
  props: [
    'columns',
    'filter',
    'view',
    'sort'
  ],
  data: function () {
    return {
      apiUrl: 'https://api.airtable.com/v0/',
      apiKey: 'keysNHLISUnwf7XKF', // Always use a read-only account token
      base: 'appPOWLf10VT6P8LX/Resources',
      records: []
    };
  },
  mounted: function () {
    this.getData();
  },
  methods: {
    getData: function () {
      axios({
        url: this.apiUrl + this.base,
        headers: {
          'Authorization': `Bearer ${this.apiKey}`
        },
        params: {
          filterByFormula: this.filter || '',
          sort: this.sort || '',
          view: this.view || ''
        }
      }).then((res) => {
        this.records = res.data.records;
      });
    }
  }
}

</script>