<template>
    <div class="mole-container" :class = "{active: isActive, inactive: !isActive}">
        <div class="mole-image-container">
        <img class="mole" src="./assets/mole.png" alt="mole" @click = "updateMole" />
        </div>
        <img class="dirt" src="./assets/dirt.svg" alt="mole dirt"/>
    </div>
</template>
<script>
export default{
    name: "mole",
    props: {
        isActive: Boolean,
        itemId: Number,
    },
    methods:{
        updateMole: function(){
            console.log(this.itemId);
            this.$emit("update-moe", this.itemId);
        },
    },
}
</script>
