<template>

<div v-if="CreateModal" class="app-modal login hidden-modal shown">
    <b-modal id="bv-modal-example" hide-footer>
    <template #modal-title>
      Using <code>$bvModal</code> Methods
    </template>
    <div class="d-block text-center">
      <h3>Hello From This Modal!</h3>
    </div>
    <!-- b-button class="mt-3" block @click="$bvModal.hide('bv-modal-example')">Close Me</b-button> -->
  </b-modal>
</div>
</template>

<script>
    export default {
    	data: function() {
            return {
                CreateModal: false
            }
        }
    }
</script>