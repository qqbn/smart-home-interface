<template>
    <div class="bright-box">
        <div class="left-box">
            <h4>Jasność</h4>
            <p>{{ this.data }}</p>
        </div>
        <div class="center-box">
            <button @click="changeBright(-1)">
                <img src="../assets/minus.png" alt="minus" />
            </button>
            <button @click="changeBright(1)">
                <img src="../assets/plus.png" alt="plus" />
            </button>
        </div>
        <div class="right-box">
            <button @click="setBrightness(this.data)">Ustaw</button>
        </div>
    </div>
</template>

<script lang="ts">
export default {
    props: {
        data: {
            required: true,
            type: Number,
        },
    },
    data() {
        return {
            counter: 0 as number | undefined,
        };
    },
    methods: {
        changeBright(n: number): void {
            //funkcja zmieniająca wartość jasności żarówki o + lub - 1
            (this as any).$emit("changeBright", n);
        },
        setBrightness(n: number): void {
            //tu powinno zostać wykonane żądanie z UPDATE do serwera z nową wartością jasności
            console.log(n);
        },
    },
};
</script>

<style scoped>
.bright-box {
    display: flex;
    justify-content: center;
    align-items: center;
    flex-direction: row;
    min-height: 80px !important;
    width: 100%;
}
.left-box {
    display: flex;
    justify-content: center;
    align-items: center;
    flex-direction: column;
    width: 40%;
    height: 100%;
    padding: 5px 20px;
}
.center-box {
    width: 20%;
    height: 100%;
    display: flex;
    justify-content: space-around;
    align-items: center;
    flex-direction: row;
}
.center-box button {
    width: 30px;
    height: 30px;
    border: none;
    font-size: 15px;
    background-color: #43e97b;
    border-radius: 50%;
    display: flex;
    justify-content: center;
    align-items: center;
    margin-bottom: 5px;
    cursor: pointer;
}
.center-box button:hover {
    background-color: #3ed16f;
}
.center-box button img {
    width: 50%;
    height: 50%;
}
.right-box {
    width: 40%;
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
h4 {
    color: #43e97b;
}
@media only screen and (max-width: 500px) {
    .bright-box {
        flex-direction: column;
    }
    .left-box {
        padding: 0px 0px;
        display: flex;
        justify-content: center;
        align-items: center;
        width: 100%;
    }
    .center-box {
        padding: 5px 0px;
        width: 100%;
        display: flex;
        justify-content: center;
        align-items: center;
    }
}
</style>

