<template>
  <div class="input-box">
    <label for="zipCode" class="label-bold">Zip-code</label>
    <input
      v-model="input"
      name="zipCode"
      class="input-field border-rounded bg-light-gray"
      required
    />
    <div v-if="errors.length <= 0">
      <span class="error-message"></span>
    </div>
    <div v-else class="padding-05">
      <span v-for="error in errors" :key="error" class="error-message">{{
        error
      }}</span>
    </div>
  </div>
</template>

<style scoped></style>

<script lang="ts">
import { minLength, maxLength, required } from '@/validators';
import { Ref, ref, watch } from 'vue';
import state from '@/state';
import useInputErrors from '@/modules/useInputErrors';

export default {
  setup() {
    const componentName = 'InputZip';

    const errors: Ref<Array<string | null>> = ref([]);
    const validators = [minLength(3), maxLength(30), required()];
    const { addError } = useInputErrors();
    const input: Ref<string> = ref(state.userForm.address.zipCode.toString());

    function doesHaveErrors(errorList: Array<string | null>) {
      errorList.forEach(error => {
        if (error !== null) addError(componentName, error);
      });
    }

    watch(state.isFormSubmitTriggered, () => {
      errors.value == null;
      errors.value = validators.map(validator => validator(input.value));
      doesHaveErrors(errors.value);
      if (state.errorList.value.length === 0) {
        state.userForm.address.zipCode = parseInt(input.value);
      }
    });
    return {
      input,
      errors,
    };
  },
};
</script>
