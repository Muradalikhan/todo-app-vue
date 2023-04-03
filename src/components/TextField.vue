<template>
    <div>
        <label v-if="label" :for="name">{{ label }}</label>
        <input :id="name" :name="name" :placeholder="placeholder" :type="type" :value="modelValue"
            @input="$emit('update:modelValue', $event.target.value)" />
    </div>
</template>
  
<script>
export default {
    name: "TextField",
    props: {
        label: {
            type: [String, Boolean],
            default: false,
        },
        name: {
            type: String,
            required: true,
        },
        placeholder: {
            type: String,
        },
        type: {
            type: String,
            default: "text",
        },
        modelValue: {
            type: [String, Number],
            default: "",
        },
    },
};
</script>
  