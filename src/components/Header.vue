<template>
    <div class="header">
        <h1>SMART HOME INTERFACE</h1>
    </div>
</template>

<script lang="ts">
export default {};
</script>

<style scoped>
@import url("https://fonts.googleapis.com/css2?family=Montserrat:wght@300;400;600;700&display=swap");
.header {
    width: 100%;
    height: 30%;
    display: flex;
    justify-content: center;
    align-items: center;
    font-weight: bold;
    padding: 20px 20px;
    margin-top: 20px;
    color: #43e97b;
    font-size: 24px;
}

@media only screen and (max-width: 760px) {
    h1 {
        font-size: 32px !important;
    }
}

@media only screen and (max-width: 490px) {
    h1 {
        font-size: 24px !important;
    }
}

@media only screen and (max-width: 380px) {
    .header {
        font-size: 22px !important;
        text-align: center;
    }
}
</style>

