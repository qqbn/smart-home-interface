<template>
    <div class="body">
        <Header />
        <Content :SmartDevices="SmartDevices" @deviceId="setDevice($event)" />
        <Dialog
            v-if="dialog"
            @closeDialog="closeDialog($event)"
            :deviceToDisplay="deviceToDisplay"
            @changeBright="changeBright($event)"
            @disconnect="disconnect($event)"
            @connect="connect($event)"
            @color="setColor($event)"
            @tempChange="tempChange($event)"
        />
    </div>
</template>

<script lang="ts">
import { defineComponent } from "vue";
import Header from "./components/Header.vue";
import Content from "./components/Content.vue";
import Dialog from "./components/Dialog.vue";

export default defineComponent({
    name: "App",
    components: { Header, Content, Dialog },
    emits: [
        "deviceId",
        "closeDialog",
        "changeBright",
        "disconnect",
        "connect",
        "color",
        "tempChange",
    ],
    data() {
        return {
            dialog: false as boolean, //zmienna sprawdzająca czy okno dialogowe jest otwarte czy zamknięte
            deviceToDisplay: {}, //obiekt, przechowujący dane aktualnie wyświetlanego urządzenia w oknie dialogowym
            timer: 0 as number, //timer do zmiany stanu połączenia
            connection: null as any,
            SmartBulb: {
                //obiekt SmartBulb
                type: "Bulb" as string,
                id: "bulb" as string,
                name: "Żarówka" as string,
                connectionState: "Connected" as string,
                isTurnedOn: true as boolean,
                brightness: 100 as number,
                color: "#03fc3d" as string,
                iconGreen: "bulb-green" as string,
            },
            SmartOutlet: {
                //obiekt SmartOutlet
                type: "Outlet" as string,
                id: "outlet" as string,
                name: "Gniazdko salon",
                connectionState: "Connected" as string,
                isTurnedOn: true as boolean,
                powerConsumption: 20 as number,
                iconGreen: "outlet-green" as string,
            },
            SmartTemperatureSensor: {
                //obiekt SmartTemperatureSensor
                type: "temperatureSensor" as string,
                id: "tempSens" as string,
                name: "Termometr salon" as string,
                connectionState: "Connected" as string,
                temperature: 18 as number,
                iconGreen: "temperature-thermometer" as string,
            },
            SmartDevices: [
                //tablica obiektów, która wyświetlana jest na liście urządzeń
                {
                    type: "Bulb" as string,
                    id: "bulb" as string,
                    name: "Żarówka" as string,
                    connectionState: "Connected" as string,
                    iconWhite: "bulb-white" as string,
                    iconGreen: "bulb-green" as string,
                },
                {
                    type: "Outlet" as string,
                    id: "outlet" as string,
                    name: "Gniazdko salon" as string,
                    connectionState: "Connected" as string,
                    iconWhite: "outlet-white" as string,
                    iconGreen: "outlet-green" as string,
                },
                {
                    type: "TemperatureSensor" as string,
                    id: "tempSens" as string,
                    name: "Termometr salon" as string,
                    connectionState: "Connected" as string,
                    iconWhite: "temperature-thermometer-white" as string,
                    iconGreen: "temperature-thermometer" as string,
                },
            ],
        };
    },
    methods: {
        setDevice(n: string): string {
            if (n == this.SmartBulb.id) {
                this.deviceToDisplay = this.SmartBulb;
            } else if (n == this.SmartOutlet.id) {
                this.deviceToDisplay = this.SmartOutlet;
            } else {
                this.deviceToDisplay = this.SmartTemperatureSensor;
            }
            this.dialog = true;
            /*
            fetch('GET /api/v1/devices/{n}')
            .then(res => res.json())
            .then(data => this.SmartDevices=data.results)
            .catch(err => console.log(err.message));
            Funkcja ta pozwala na otworzenie okna dialogowego dla danego urządzenia,
            z dostępem do bazy danych oraz dostępme do serwera aplikacji, tutaj wykonywane by było żądanie zwracające dane
            pojedyńczego urządzenia, aktualne dane zostałyby przypisane do zmiennej deviceToDisplay a następnie wyświetlone w oknie dialogowym.  
            */
            return n;
        },

        disconnect(n: string): void {
            //na potrzeby interaktywnego testowania aplikacji, funkcja ta zmienia stan urządzenia z connected na disconnected w oknie dialogowym
            //jak i na liście urządzeń
            if (n == "bulb") {
                this.SmartBulb.connectionState = "Disconnected";
            } else if (n == "outlet") {
                this.SmartOutlet.connectionState = "Disconnected";
            } else {
                this.SmartTemperatureSensor.connectionState = "Disconnected";
            }

            this.SmartDevices.forEach((element) => {
                if (element.id == n) {
                    element.connectionState = "Disconnected";
                }
            });
        },

        connect(n: string): void {
            //na potrzeby interaktywnego testowania aplikacji, funkcja ta zmienia stan urządzenia z disconnected na connected w oknie dialogowy
            //jak i na liście urządzeń
            if (n == "bulb") {
                this.SmartBulb.connectionState = "Connected";
            } else if (n == "outlet") {
                this.SmartOutlet.connectionState = "Connected";
            } else {
                this.SmartTemperatureSensor.connectionState = "Connected";
            }

            this.SmartDevices.forEach((element) => {
                if (element.id == n) {
                    element.connectionState = "Connected";
                }
            });
        },
        closeDialog(n: boolean): void {
            //funkcja zamykająca okno dialogowe
            this.dialog = n;
        },

        tempChange(n: number): void {
            //funkcja zmieniająca wartość temperatury na termometrze o + lub - 1
            this.SmartTemperatureSensor.temperature += n;
        },

        changeBright(n: number): void {
            //funkcja zmieniająca jasność żarówki o + lub - 1
            this.SmartBulb.brightness += n;
            if (this.SmartBulb.brightness > 100) {
                this.SmartBulb.brightness = 100;
            } else if (this.SmartBulb.brightness < 0) {
                this.SmartBulb.brightness = 0;
            }
        },

        setColor(n: string): void {
            //ustawienie koloru żarówki
            this.SmartBulb.color = n;
        },

        changeState(): void {
            //symulacja zmiany połączenia elementów, wykorzystująca funkcję setInterval, urządzenia zmieniają swoje połączenie z connected na poor-connection co 30sek
            this.SmartDevices.forEach((element) => {
                if (element.connectionState === "Connected") {
                    element.connectionState = "Poor Connection";
                    if (element.id === "bulb") {
                        this.SmartBulb.connectionState = "Poor Connection";
                    } else if (element.id === "outlet") {
                        this.SmartOutlet.connectionState = "Poor Connection";
                    } else {
                        this.SmartTemperatureSensor.connectionState =
                            "Poor Connection";
                    }
                } else if (element.connectionState === "Poor Connection") {
                    element.connectionState = "Connected";
                    if (element.id === "bulb") {
                        this.SmartBulb.connectionState = "Connected";
                    } else if (element.id === "outlet") {
                        this.SmartOutlet.connectionState = "Connected";
                    } else {
                        this.SmartTemperatureSensor.connectionState =
                            "Connected";
                    }
                }
            });
        },
        cancelAutoUpdate(): void {
            clearInterval(this.timer);
        },
        /*getData(): void{
            fetch('GET /api/v1/devices')
            .then(res => res.json())
            .then(data => this.SmartDevices=data.results)
            .catch(err => console.log(err.message));
        }
        prototyp funkcji do wywołania w mounted, która na starcie aplikacji pobiera dane wszystkich urządzeń, a następnie wyświetli urządzenia na liście
        */
    },
    mounted(): void {
        // this.getData();
    },
    created(): void {
        this.timer = setInterval(this.changeState, 30000);
        /*
        Prototyp aktualizowania stanu urządzeń przy użyciu websocket
        this.connection = new WebSocket("wss://GET /api/v1/refresh"); //użycie websocket
        this.connection.onopen = function (event: any): void {
            //funkcja wypisująca 'Połączenie' w konsoli, gdy aplikacja połączy się poprzez websocket
            console.log("Połączenie");
        };

        this.connection.onmessage = function (event: any): void { //użycie onmessage 
            this.SmartDevices.forEach(element => { //dane zwrócone poprzez onmessage, to SmartDeviceDetails, więc na początku, przechodzę pętlą poprzez tablicę
                                                    //wszystkich urządzeń na liście, aby zmienić stan urządzenia o tym samym ID co zwrócone urządzenie
                if(element.id===event.data.id){
                    element.connectionState=event.data.connectionState;
                }

            });

            //następnie sprawdzam które urządzenie zostało, zwrócone tak aby przypisać nowe wartości do urządzenia
            if(this.SmartBulb.id===event.data.id){
                this.SmartBulb=event.data;
            }else if(this.SmartOutlet.id===event.data.id){
                this.SmartOutlet=event.data;
            }else{
                this.SmartTemperatureSensor=event.data;
            }
        };

        */
    },
    beforeUnmount(): void {
        this.cancelAutoUpdate();
    },
});
</script>

<style>
@import url("https://fonts.googleapis.com/css2?family=Montserrat:wght@300;400;600;700&display=swap");
* {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
}
#app {
    font-family: "Montserrat", sans-serif;
}
.body {
    width: 100%;
    height: 100%;
    display: flex;
    justify-content: space-around;
    align-content: center;
    flex-direction: column;
}
</style>
