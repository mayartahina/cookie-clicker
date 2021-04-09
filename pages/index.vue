<template>
  <div>
    <v-card class="text-center pa-5">
      <v-card-title>Counter: {{ counter }}</v-card-title>

      <v-btn @click="incr()" color="primary">Add (1) to Counter </v-btn>
    </v-card>
  </div>
</template>

<script>
export default {
  data: () => ({
    counter: 0
  }),
  mounted() {
    this.$fire.firestore
      .collection("counter")
      .doc("counter")
      .get()
      .then(result => {
        if (!result.exists)
          this.$fire.firestore
            .collection("counter")
            .doc("counter")
            .set({ count: 0 });
      });
    this.$fire.firestore
      .collection("counter")
      .doc("counter")
      .onSnapshot(counter => {
        const data = counter.data();
        this.counter = data.count;
      });
  },
  methods: {
    incr() {
      this.$fire.firestore
        .collection("counter")
        .doc("counter")
        .update("count", this.$fireModule.firestore.FieldValue.increment(1));
    }
  }
};
</script>
