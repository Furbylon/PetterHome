<template>
  <nb-content>
    <nb-card>
      <nb-card>
        <nb-card-item>
          <nb-body>
            <nb-Label>
              Owner: {{ receivedPet.user.username }}</nb-Label
            ></nb-body
          >
        </nb-card-item>
      </nb-card>
      <nb-card-item
        button
        :on-press="() => navigateToPet(receivedPet.pet.id, receivedPet.user.id)"
      >
        <nb-body>
          <nb-text>Name: {{ receivedPet.pet.petName }}</nb-text>
          <nb-text>Animal: {{ receivedPet.pet.animal.animal }}</nb-text>
          <nb-text>Age: {{ receivedPet.pet.petAge }}</nb-text>
          <nb-text>Gender: {{ receivedPet.pet.gender }}</nb-text>
          <nb-text>Breed: {{ receivedPet.pet.animal.breed }}</nb-text>
        </nb-body>
        <nb-button
          rounded class="button"
          :on-press="() => navigateToPetNeeds(receivedPet.pet.id)"
          ><nb-text>Schedules</nb-text>
        </nb-button>
      </nb-card-item>
    </nb-card>
  </nb-content>
</template>

<script>
export default {
  props: {
    receivedPet: {
      type: Object,
      default: () => {},
    },
    navigateToPet: {
      type: Function,
    },
    navigateToPetNeeds: {
      type: Function,
    },
  },
};
</script>
<style scoped>
.button {
  background-color: #edccaf;
}
</style>