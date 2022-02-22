<template>
    <div class="color-box">
        <div class="left-box">
            <h4>Kolor</h4>
            <div class="color" :style="{ background: color }"></div>
        </div>
        <div class="center-box">
            <h4>Wprowadź kolor HEX</h4>
            <input type="text" name="color" id="color" v-model="this.color" />
        </div>
        <div class="right-box">
            <button @click="setColor(this.color)">Ustaw</button>
        </div>
    </div>
</template>

<script>
export default {
    props: {
        data: String,
    },
    data() {
        return {
            color: "",
        };
    },
    methods: {
        setColor(n) {
            //funkcja ustawiajaca kolor
            this.color = n;
            this.$emit("color", this.color);
        },
    },
    mounted() {
        this.color = this.data;
    },
};
</script>

<style scoped>
.color-box {
    width: 100%;
    min-height: 80px !important;
    display: flex;
    justify-content: center;
    align-items: center;
    flex-direction: row;
}
h4 {
    color: #43e97b;
}
.left-box {
    width: 25%;
    height: 100%;
    display: flex;
    justify-content: space-around;
    align-items: center;
    flex-direction: column;
}
.center-box {
    width: 50%;
    height: 100%;
    display: flex;
    justify-content: space-around;
    align-items: center;
    flex-direction: column;
}
.center-box input {
    width: 50%;
    height: 50%;
    font-size: 16px;
    border: none;
    border-radius: 20px;
    text-align: center;
}
.center-box input:focus {
    outline: none;
}
.right-box {
    width: 25%;
    height: 100%;
    display: flex;
    justify-content: center;
    align-items: center;
}
.right-box button {
    background: transparent;
    border: none;
    background: rgb(242, 242, 242);
    background: linear-gradient(
        90deg,
        rgba(242, 242, 242, 1) 52%,
        rgba(67, 233, 123, 1) 78%,
        rgba(56, 249, 215, 1) 100%
    );
    background-repeat: no-repeat;
    background-position: left;
    transition: 0.5s all;
    background-size: 400% 400%;
    cursor: pointer;
    font-size: 16px;
    height: 40px;
    width: 80px;
    border-radius: 20px;
}
.right-box button:hover {
    background-position: 100%;
    -webkit-box-shadow: -0px 15px 19px -15px rgba(4, 4, 4, 0.26);
    -moz-box-shadow: -0px 15px 19px -15px rgba(4, 4, 4, 0.26);
    box-shadow: -0px 15px 19px -15px rgba(4, 4, 4, 0.26);
    color: white;
}
.color {
    width: 40px;
    height: 40px;
    border-radius: 50%;
}
@media only screen and (max-width: 500px) {
    .color-box {
        flex-direction: column;
    }
    .center-box {
        display: flex;
        align-items: center;
        justify-content: space-around;
        height: 70px;
        width: 100%;
    }
    .center-box h4 {
        width: 100%;
        text-align: center;
    }
}
</style>