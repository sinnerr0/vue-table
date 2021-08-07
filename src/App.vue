<template>
  <v-app>
    <v-main>
      <v-radio-group v-model="select" :column="false">
        <v-radio label="Object" value="Object"></v-radio>
        <v-radio label="Set" value="Set"></v-radio>
      </v-radio-group>
      <TableObject v-if="select === 'Object'" :items="items" />
      <TableSet v-else :items="items" />
    </v-main>
  </v-app>
</template>

<script>
import TableObject from './components/TableObject'
import TableSet from './components/TableSet'
import faker from 'faker'

export default {
  name: 'App',
  components: {
    TableObject,
    TableSet,
  },
  data() {
    return {
      select: 'Set',
      items: [],
    }
  },
  created() {
    const items = []
    for (let i = 0; i < 400; i++) {
      items.push({
        id: i,
        name: faker.name.findName(),
        phone: faker.phone.phoneNumber(),
        country: faker.address.country(),
        isSelected: false,
      })
    }
    this.items = items
  },
}
</script>

<style lang="scss">
table {
  font-family: arial, sans-serif;
  border-collapse: collapse;
  width: 100%;
  content-visibility: auto;
}

td,
th {
  border: 1px solid #dddddd;
  text-align: left;
  padding: 8px;

  &:first-child {
    text-align: center;
  }
}

tr:nth-child(even) {
  background-color: #dddddd;
}
</style>
