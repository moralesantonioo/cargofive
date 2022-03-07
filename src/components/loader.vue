<template>
    <div>
        <b-overlay 
        :show="show" 
        spinner-variant="primary"
        ></b-overlay>
    </div>
</template>

<script>
export default {
    props:{
        show: {type: Boolean, default: false}
    }
}
</script>

<style>

</style>