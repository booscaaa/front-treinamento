<template>
  <div>
    <v-card class="mb-5">
      <v-card-title>
        {{ item.descricao }}
        <v-spacer></v-spacer>
        <v-icon color="blue">mdi-pencil</v-icon>
        <v-icon color="red">mdi-delete</v-icon>
      </v-card-title>
      <v-card-text>
        {{ item.data }}
      </v-card-text>
    </v-card>
  </div>
</template>

<script>
export default {
  props: {
    item: {
      type: Object,
      required: true,
    },
  },

  data: () => ({
    dialog: false,
  })
};
</script>
