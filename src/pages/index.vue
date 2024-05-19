<template>
  <v-app id="inspire">
    <v-main>
      <v-container>
        <HeatMapChat />
      </v-container>
    </v-main>
  </v-app>
</template>

<script lang="ts" setup>

</script>
<style lang="scss">
::-webkit-scrollbar {
  width: 10px;
  height: 8px;
  background-color: #fff;
  border: 1px solid #e6e8f0;
  border-radius: 20px;
}

::-webkit-scrollbar-thumb {
  background-color: #c3c3c5;
  border-radius: 20px;
}

::-webkit-scrollbar-track {
  width: 15px;
  background-color: #f5f5f5;

}

::-webkit-scrollbar-thumb:hover {
  background-color: #adadad;
}
</style>
<style lang="scss" scoped></style>
