<template>
    <button @click="handleClick">{{ label }}</button>
</template>

<script>
export default {
    name: "AppButton",
    props: {
        label: String,
        handleClick: Function
    }
}
</script>

<style scoped>
button {
    background-color: yellowgreen;
    border-radius: 8px;
    padding: 10px;
    font-weight: 600;
    width: 100px;
    border: 1px solid black;

}
</style>