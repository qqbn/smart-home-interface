<template>
    <div ref="myDialog" class="dialog">
        <div class="img-box">
            <img
                :src="require(`@/assets/${deviceToDisplay.iconGreen}.svg`)"
                alt="temp"
            />
        </div>
        <div class="data-box">
            <div class="data">
                <h4>Typ urządzenia</h4>
                <p>{{ deviceToDisplay.type }}</p>
            </div>
            <div class="data">
                <h4>Nazwa</h4>
                <p>{{ deviceToDisplay.name }}</p>
            </div>
            <div class="data-connection">
                <div class="left-box">
                    <h4>Połączenie</h4>
                    <p>{{ deviceToDisplay.connectionState }}</p>
                </div>
                <div class="right-box">
                    <button
                        @click="disconnect(this.deviceToDisplay.id)"
                        v-show="
                            isConnected(this.deviceToDisplay.connectionState)
                        "
                    >
                        Rozłącz
                    </button>
                    <button
                        v-show="
                            !isConnected(this.deviceToDisplay.connectionState)
                        "
                        @click="connect(this.deviceToDisplay.id)"
                    >
                        Połącz
                    </button>
                </div>
            </div>
            <div class="data">
                <h4>Status</h4>
                <p v-show="deviceToDisplay.isTurnedOn">Włączony</p>
                <p v-show="!deviceToDisplay.isTurnedOn">Wyłączony</p>
            </div>
            <div class="data" v-if="isBulb(deviceToDisplay.brightness)">
                <Brightness
                    :data="deviceToDisplay.brightness"
                    @changeBright="changeBright($event)"
                />
            </div>
            <div class="data" v-if="isBulb(deviceToDisplay.color)">
                <BulbColor
                    :data="deviceToDisplay.color"
                    @color="setColor($event)"
                />
            </div>
            <div class="data" v-if="isOutlet(deviceToDisplay.powerConsumption)">
                <OutletPowerCons :data="deviceToDisplay.powerConsumption" />
            </div>
            <div class="data" v-if="isThermo(deviceToDisplay.temperature)">
                <Temperature
                    :data="deviceToDisplay.temperature"
                    @tempChange="tempChange($event)"
                />
            </div>
            <div class="data">
                <button @click="closeDialog(false)" class="close-dialog">
                    Zamknij
                </button>
            </div>
        </div>
    </div>
</template>

<script>
import Brightness from "./Brightness.vue";
import BulbColor from "./BulbColor.vue";
import OutletPowerCons from "./OutletPowerCons.vue";
import Temperature from "./Temperature.vue";
import interact from "interactjs";
export default {
    components: { Brightness, BulbColor, OutletPowerCons, Temperature },
    props: ["deviceToDisplay"],
    data() {
        return {
            position: {
                x: 0,
                y: 0,
            },
            size: {
                width: 0,
                height: 0,
            },
        };
    },
    methods: {
        closeDialog(n) {
            //funkcja wysyłająca wartość zmiennej do komponentu "matki" aby dialog został zamknięty
            this.$emit("closeDialog", n);
        },

        disconnect(n) {
            //funkcja wysyłająca id urządzenia do komponentu "matki", aby została zmieniona wartość połączenia z connnected na disconnected
            this.$emit("disconnect", n);
            //przy dostępnej bazie danych oraz dostępnym serwerze aplikacji, funkcja ta wysyłałaby żądanie, aby rozłączyć urządzenie
        },

        connect(n) {
            //funkcja wysyłająca id urządzenia do komponentu "matki", aby została zmieniona wartość połączenia z disconnected na connected
            this.$emit("connect", n);
            //przy dostępnej bazie danych oraz dostępnym serwerze aplikacji, funkcja ta wysyłałaby żądanie, aby połączyć urządzenie
        },

        setColor(n) {
            //funkcja wysyłająca wartość koloru(string HEX) do komponentu "matki", aby został zmieniony kolor
            this.$emit("color", n);
        },

        isConnected(n) {
            //sprawdzenie czy urządzenie ma status disconnected
            if (n === "Disconnected") {
                return false;
            }
            return true;
        },

        isBulb(n) {
            //sprawdzenie czy urządzenie, które jest wyświetlane jest żarówką oraz czy posiada dane atrybuty
            if (typeof n === "number" || typeof n === "string") {
                return true;
            }
            return false;
        },

        isOutlet(n) {
            //sprawdzenie czy urządzenie, które jest wyświetlane jest gniazdkiem oraz czy posiada dane atrybuty
            if (typeof n === "number") {
                return true;
            }
            return false;
        },

        isThermo(n) {
            //sprawdzenie czy urządzenie, które jest wyświetlane jest termometrem oraz czy posiada dane atrybuty
            if (typeof n === "number") {
                return true;
            }
            return false;
        },

        changeBright(n) {
            //przekazanie danych do zmiany jasności żarówki
            this.$emit("changeBright", n);
        },

        tempChange(n) {
            //przekazanie danych do zmiany temperatury
            this.$emit("tempChange", n);
        },

        //W tej sekcji umieściłem kod związany z biblioteką interact.js
        initInteract: function (selector) {
            interact(selector)
                .draggable({
                    inertia: true,
                    modifiers: [
                        interact.modifiers.restrictRect({
                            restriction: "parent",
                            endOnly: true,
                        }),
                    ],
                    autoScroll: true,
                    listeners: {
                        // start: this.dragStartListener,
                        move: this.dragMoveListener,
                        end: this.onEndListener,
                    },
                })
                .resizable({
                    // resize from all edges and corners
                    edges: {
                        left: false,
                        right: true,
                        bottom: false,
                        top: true,
                    },

                    listeners: {
                        move: this.resizeMoveListener,
                        end: this.setSize,
                    },
                    modifiers: [
                        // keep the edges inside the parent
                        interact.modifiers.restrictEdges({
                            outer: "parent",
                        }),

                        // minimum size
                        interact.modifiers.restrictSize({
                            min: { width: 350, height: 550 },
                            max: { width: 650, height: 700 },
                        }),
                    ],

                    inertia: true,
                });
        },

        resizeMoveListener(event) {
            //listener odpowiadający za zmianę szerokości oraz wysokości okna dialogowego
            var target = event.target;

            // update the element's style
            target.style.width = event.rect.width + "px";
            target.style.height = event.rect.height + "px";
        },

        setSize(event) {
            //listener, który na zakończenie zapisuje dane wysokości i szerokości okna dialogowego w localstorage, tak aby wyświetlało się okno o rozmiarach zamkniętego okna
            var target = event.target;

            this.size.width = target.style.width;
            this.size.height = target.style.height;

            const parsed = JSON.stringify(this.size);
            localStorage.setItem("size", parsed);
        },

        dragStartListener: function (event) {
            var target = event.target;
            if (this.position.x == 0 && this.position.y == 0) {
                target.style.transform = "translateX(50%)";
            }
        },

        dragMoveListener: function (event) {
            //listener odpowiadający za przemieszczanie się okna dialogowego opcją dragging
            var target = event.target;

            // keep the dragged position in the data-x/data-y attributes
            var x =
                (parseFloat(target.getAttribute("data-x")) || this.position.x) +
                event.dx;
            var y =
                (parseFloat(target.getAttribute("data-y")) || this.position.y) +
                event.dy;

            // translate the element
            target.style.transform = "translate(" + x + "px, " + y + "px)";

            // update the posiion attributes
            target.setAttribute("data-x", x);
            target.setAttribute("data-y", y);
        },

        onEndListener: function (event) {
            //listener zapamiętujący ostatnią pozycję okna dialogowego, dane zapisywane są w localstorage, tak aby po zamknięciu okna dialogowego, wyświetliło się ono
            //w ostatnio zamkniętym miejscu
            var target = event.target;

            this.position.x = parseFloat(target.getAttribute("data-x"));
            this.position.y = parseFloat(target.getAttribute("data-y"));

            const parsed = JSON.stringify(this.position);
            localStorage.setItem("position", parsed);
        },

        getPosition(dialog) {
            //funkcja, która przy otworzeniu okna dialogowego, pobiera dane ostatniej pozycji, oraz przypisuje mu te dane, wyświetlając okno dialogowe w ostatnim miejscu,
            //gdzie zostało zamknięte
            if (localStorage.getItem("position")) {
                this.position = JSON.parse(localStorage.getItem("position"));
                dialog.style.transform =
                    "translate(" +
                    this.position.x +
                    "px, " +
                    this.position.y +
                    "px)";
            }
        },

        getSize(dialog) {
            //funkcja która, przy otworzeniu okna dialogowego, pobiera dane ostatniego rozmiaru okna oraz przypisuje mu te dane, wyświetlając okno w rozmiarach, w których
            //zostało ostatnio zamknięte
            if (localStorage.getItem("size")) {
                this.size = JSON.parse(localStorage.getItem("size"));
                dialog.style.width = this.size.width;
                dialog.style.height = this.size.height;
            }
        },
    },
    mounted() {
        //inicjalizacja bibliotekii interact.js oraz wywołanie funkcji
        let myDialog = this.$refs.myDialog;
        this.initInteract(myDialog);

        var dialog = document.querySelector(".dialog");

        this.getPosition(dialog);
        this.getSize(dialog);
    },
};
</script>


<style scoped>
.dialog {
    width: 500px;
    height: 600px;
    border-radius: 20px;
    left: 50%;
    transform: translateX(-50%);
    position: absolute;
    background-color: #f2f2f2;
    -webkit-box-shadow: 0px 15px 12px 0px rgba(35, 35, 35, 0.45);
    -moz-box-shadow: 0px 15px 12px 0px rgba(35, 35, 35, 0.45);
    box-shadow: 0px 15px 12px 0px rgba(35, 35, 35, 0.45);
    touch-action: none;
    user-select: none;
}
.img-box {
    height: 30%;
    width: 100%;
    border-top-left-radius: 20px;
    border-top-right-radius: 20px;
    display: flex;
    justify-content: center;
    align-items: center;
    padding: 20px 0px;
}
.data-box {
    height: 70%;
    width: 100%;
    border-bottom-left-radius: 20px;
    border-bottom-right-radius: 20px;
    overflow: auto;
    display: flex;
    justify-content: space-around;
    align-items: center;
    flex-direction: column;
}
.data {
    padding: 0px 0px;
    display: flex;
    justify-content: space-around;
    align-items: center;
    flex-direction: column;
    width: 100%;
    min-height: 50px;
}
.data h4 {
    color: #43e97b;
}
.data p {
    padding: 10px 0px;
}
.data-connection {
    padding: 0px 0px;
    display: flex;
    justify-content: space-around;
    align-items: center;
    flex-direction: row;
    width: 100%;
    min-height: 50px;
}
.left-box {
    width: 50%;
    height: 100%;
    display: flex;
    justify-content: space-around;
    align-items: center;
    flex-direction: column;
}
.left-box h4 {
    color: #43e97b;
}
.right-box {
    width: 50%;
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
.close-dialog {
    height: 40px;
    width: 80px;
    border-radius: 20px;
    border: none;
    font-size: 16px;
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
}
.close-dialog:hover {
    background-position: 100%;
    -webkit-box-shadow: -0px 15px 19px -15px rgba(4, 4, 4, 0.26);
    -moz-box-shadow: -0px 15px 19px -15px rgba(4, 4, 4, 0.26);
    box-shadow: -0px 15px 19px -15px rgba(4, 4, 4, 0.26);
    color: white;
}
@media only screen and (max-width: 500px) {
    .dialog {
        width: 90% !important;
        height: auto !important;
    }
}

@media only screen and (max-width: 360px) {
    .data-connection {
        flex-direction: column;
    }
    .dialog {
        width: 100% !important;
    }
}
</style>