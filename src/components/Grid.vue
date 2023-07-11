<template>
    <table>
        <thead>
            <tr>
                <th v-for="key in columns">{{key}}</th>
            </tr>
        </thead>
        <tbody>
            <tr v-for="entry in data">
                <td v-for="key in columns">{{entry[key]}}</td>
            </tr>
        </tbody>
    </table>
</template>

<script>
export default {
    props: {
        data: Array,
        columns: Array,
    },
}
</script>
<style>
table {
  border: 2px solid #42b983;
  border-radius: 3px;
  background-color: #fff;
  /*center*/
  margin: 0 auto;
}

th {
  background-color: #42b983;
  color: rgba(255, 255, 255, 0.66);
  cursor: pointer;
  user-select: none;
}

td {
  background-color: #f9f9f9;
}

th,
td {
  min-width: 120px;
  padding: 10px 20px;
}
</style>