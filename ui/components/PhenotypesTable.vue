<template>
    <div>
        <b-table :items="rows"
                 :fields="fields"
                 responsive="true"
                 class="table-border-soft"
                 :current-page="currentPage"
                 :per-page="rowsPerPage"
        >
        </b-table>
        <pre>{{preItems}}</pre>
    </div>
</template>
<script>
  import * as MA from '../../js/MonarchAccess';

  export default {
    data() {
      return {
        rowsPerPage: 10,
        currentPage: 1,
        fields: [
          {
            key: 'hit',
            sortable: true,
          },
          {
            key: 'combined_score',
            sortable: true,
          },
          {
            key: 'most_informative_shared_phenotype',
            sortable: true,
          },
          {
            key: 'other_matching_phenotypes',
            sortable: true,
          },
        ],
        items: [],
        preItems: [],
      };
    },
    props: {
      phenotypes: {
        type: Array,
        required: true,
      },
    },
    mounted(){
      this.comparePhenotypes();
    },
    watch: {
      preItems(){
        this.processItems();
      },
    },
    methods: {
      async comparePhenotypes() {
        const that = this;
        try {
          let searchResponse = await MA.comparePhenotypes(this.phenotypes);
          this.preItems = searchResponse;
        }
        catch (e) {
          that.dataError = e;
          console.log('BioLink Error', e);
        }
      },
      processItems(){
        this.preItems.data.results.forEach((elem) => {
          console.log(elem);
        });
      },
    },
  }
</script>
<style>

</style>
