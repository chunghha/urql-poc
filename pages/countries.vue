<template>
  <section>
    <div style="display: flex; align-items: center">
      <input type="text" v-model.trim="code" placeholder="search code">
      <div v-if="fetching">fetching...</div>
    </div>
    <div v-if="data">
      <table v-if="data.countries.length">
        <thead>
          <tr>
            <th style="width: 2em; white-space: nowrap">Flag</th>
            <th>Name</th>
            <th style="width: 8em; white-space: nowrap">Currency</th>
            <th>Native</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="c in data.countries" :key="c.code">
            <td>{{ c.emoji }}</td>
            <td>{{ c.name }}</td>
            <td>{{ c.currency }}</td>
            <td>{{ c.native }}</td>
          </tr>
        </tbody>
      </table>
      <div v-else><em>no results</em></div>
    </div>
  </section>
</template>

<script setup lang="ts">
import { ref, computed } from "vue";
import { AllCountriesDocument } from "~/gql/queries/all-countries";
import { useClientHandle } from "@urql/vue";

const code = ref("");

const variables = computed(() => {
  return code.value ? {filter: {code: {eq: code.value.toUpperCase()}}} : {}
})

const urql = useClientHandle();

const { data, fetching } = await urql.useQuery({query: AllCountriesDocument, variables})

</script>
