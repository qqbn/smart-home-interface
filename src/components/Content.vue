<template>
    <div class="content" v-for="device in SmartDevices" :key="device.id">
        <Device :device="device" @deviceId="setDevice($event)" />
    </div>
</template>

<script>
import Device from "./Device.vue";
export default {
    props: ["SmartDevices"],
    emits: ["deviceId"],
    components: { Device },
    methods: {
        setDevice(n) {
            //funkcja przekazująca id urządzenia do komponentu "matki"
            this.$emit("deviceId", n);
        },
    },
};
</script>

<style scoped>
.content {
    width: 100%;
    height: 70%;
    display: flex;
    justify-content: space-around;
    align-items: center;
    padding: 30px 0px;
}
</style>