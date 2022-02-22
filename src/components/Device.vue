<template>
    <div class="device-box">
        <div class="img-box">
            <img
                :src="require(`@/assets/${device.iconGreen}.svg`)"
                alt="temp"
                class="svg-green"
            />
            <img
                :src="require(`@/assets/${device.iconWhite}.svg`)"
                alt="temp"
                class="svg-white"
            />
        </div>
        <div class="data-box">
            <h4>Typ urządzenia</h4>
            <p>{{ device.type }}</p>
        </div>
        <div class="data-box">
            <h4>Nazwa</h4>
            <p>{{ device.name }}</p>
        </div>
        <div class="data-box">
            <h4>Połączenie</h4>
            <p>{{ device.connectionState }}</p>
        </div>
        <div class="button-box">
            <button @click="openDialog(device.id)">Więcej</button>
        </div>
    </div>
</template>

<script lang="ts">
export default {
    props: ["device"],
    data() {
        return {};
    },
    methods: {
        openDialog(n: string): void {
            //funkcja wysyłająca do komponentu "matki", id dialogu, który musi zostać wyświetlony
            (this as any).$emit("deviceId", n);
        },
    },
};
</script>

<style scoped>
.device-box {
    width: 80%;
    height: 150px;
    border-radius: 20px;
    -webkit-box-shadow: -0px 15px 19px -15px rgba(4, 4, 4, 0.26);
    -moz-box-shadow: -0px 15px 19px -15px rgba(4, 4, 4, 0.26);
    box-shadow: -0px 15px 19px -15px rgba(4, 4, 4, 0.26);
    display: flex;
    justify-content: center;
    align-items: center;
    /* background-color: #f2f2f2; */
    /* background-image: linear-gradient(to right, #43e97b 0%, #38f9d7 100%); */
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
}
.img-box {
    width: 15%;
    height: 100%;
    display: flex;
    justify-content: center;
    align-items: center;
    border-top-left-radius: 20px;
    border-bottom-left-radius: 20px;
    background-position: center;
    background-repeat: no-repeat;
    background-size: contain;
}
.data-box {
    width: 23%;
    height: 100%;
    display: flex;
    justify-content: space-around;
    align-items: center;
    flex-direction: column;
}
.button-box {
    width: 15%;
    height: 100%;
    display: flex;
    justify-content: center;
    align-items: center;
}

.button-box button {
    background-color: transparent;
    border: none;
    height: 50px;
    width: 100px;
    font-size: 18px;
    color: #43e97b;
    font-weight: bold;
    cursor: pointer;
}
h4 {
    color: #43e97b;
}
.device-box:hover {
    background-position: 100%;
    height: 160px;
    -webkit-box-shadow: 0px 15px 12px 0px rgba(35, 35, 35, 0.45);
    -moz-box-shadow: 0px 15px 12px 0px rgba(35, 35, 35, 0.45);
    box-shadow: 0px 15px 12px 0px rgba(35, 35, 35, 0.45);
}

.device-box:hover .data-box p {
    color: white;
}

.device-box:hover .data-box h4 {
    color: white;
}

.device-box:hover .button-box button {
    color: white;
}
.svg-white {
    display: none;
}
.svg-green {
    display: flex;
}
.device-box:hover .svg-white {
    display: flex;
}

.device-box:hover .svg-green {
    display: none;
}
@media only screen and (max-width: 1050px) {
    .svg-green {
        width: 80%;
        height: 80%;
    }
    .svg-white {
        width: 80%;
        height: 80%;
    }
}
@media only screen and (max-width: 760px) {
    .device-box {
        min-height: 400px;
        width: 90%;
        flex-direction: column;
    }
    .img-box {
        width: 100%;
        padding: 20px 10px;
    }
    .svg-green {
        width: auto;
        height: auto;
    }
    .svg-white {
        width: auto;
        height: auto;
    }
    .data-box {
        width: 100%;
        justify-content: center;
        flex-direction: row;
    }
    .data-box h4 {
        padding: 0px 30px;
        width: 50%;
        text-align: center;
        display: flex;
        justify-content: flex-start;
        align-items: center;
        height: 100%;
    }
    .data-box p {
        width: 50%;
        text-align: center;
        display: flex;
        justify-content: flex-start;
        align-items: center;
        height: 100%;
    }

    .device-box:hover {
        min-height: 410px;
    }
}

@media only screen and (max-width: 490px) {
    .img-box {
        height: 100px;
    }
    .svg-green {
        height: 100%;
    }
    .svg-white {
        height: 100%;
    }
    .data-box h4 {
        padding: 0px 10px;
        width: 50%;
        text-align: center;
        display: flex;
        justify-content: flex-start;
        align-items: center;
        height: 100%;
    }
    .data-box p {
        width: 50%;
        text-align: center;
        display: flex;
        justify-content: flex-start;
        align-items: center;
        height: 100%;
    }
}

@media only screen and (max-width: 380px) {
    .data-box {
        flex-direction: column;
        justify-content: center;
        align-items: center;
    }
    .data-box p {
        padding: 0;
        justify-content: center;
    }
    .data-box h4 {
        padding: 0;
        justify-content: center;
    }
}
</style>
