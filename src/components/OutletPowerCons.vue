<template>
    <div class="out-pwr">
        <h4>Zużycie energii</h4>
        <p>{{ this.data }} W</p>
    </div>
</template>

<script>
export default {
    props: {
        data: Number,
    },
};
</script>

<style scoped>
.out-pwr {
    width: 100%;
    height: 100%;
    display: flex;
    justify-content: space-around;
    align-items: center;
    flex-direction: column;
}
.out-pwr h4 {
    color: #43e97b;
}
</style>
