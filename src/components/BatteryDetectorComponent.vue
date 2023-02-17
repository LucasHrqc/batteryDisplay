<template>
    <html>

    <body>

        <section class="battery">
            <div class="Bcard">
                <div class="Bpill">
                    <div class="Blevel">
                        <div class="Bliquid"></div>
                    </div>
                </div>
                <div class="Bdata">
                    <p class="Btext">Battery :</p>
                    <h1 class="Bpercentage">{{batteryPercentage}}</h1>
                    <p class="Bstatus">
                        <i class="ri-plug-line"></i>
                    </p>
                </div>
            </div>
        </section>

    </body>

    </html>
</template>


<script>

export default {
    name: 'BatteryDetectorComponent',
    components: {},
    props: [],
    data() {
        return {
            batteryLiquid: false,
            batteryStatus: false,
            batteryPercentage: false,
        }
    },
    computed: {},
    methods: {
        initBattery() {
            navigator.getBattery().then((batt) => {
                this.updateBattery(batt)
                batt.addEventListener('chargingchange', (batt) => { this.updateBattery(batt) });
                batt.addEventListener('levelchange', (batt) => { this.updateBattery(batt) });
            })
        },
        updateBattery(batt) {
            console.log(batt);
            const batteryLiquidEl = document.querySelector('.Bliquid');
            const batteryStatusEl = document.querySelector('.Bstatus');
            let level = Math.floor(batt.level * 100);
            this.batteryPercentage = level + '%';
            batteryLiquidEl.style.height = `${parseInt(batt.level * 100)}%`;
            if (level == 100) {
                batteryStatusEl.innerHTML = 'Battery full <i class="ri-battery-2-fill green-color"></i>';
                batteryLiquidEl.style.height = '103%';
            } else if (level <= 20 & !batt.charging) {
                batteryStatusEl.innerHTML = 'Low battery <i class="ri-plug-line animated-red animated-red"></i>';
            } else if (batt.charging) {
                batteryStatusEl.innerHTML = 'Charging... <i class="ri-flashlight-line animated-green"></i>';
            } else {
                batteryStatusEl.innerHTML = '';
            }
            if (level <= 20) {
                batteryLiquidEl.classList.add('gradient-color-red');
                batteryLiquidEl.classList.remove('gradient-color-green', 'gradient-color-orange', 'gradient-color-yellow');
            } else if (level <= 48) {
                batteryLiquidEl.classList.add('gradient-color-orange');
                batteryLiquidEl.classList.remove('gradient-color-green', 'gradient-color-red', 'gradient-color-yellow');
            } else if (level <= 80) {
                batteryLiquidEl.classList.add('gradient-color-green');
                batteryLiquidEl.classList.remove('gradient-color-orange', 'gradient-color-red', 'gradient-color-yellow');
            } else {
                batteryLiquidEl.classList.add('gradient-color-green');
            }
        }
    },
    created() { },
    mounted() {
        this.initBattery()
    },
}


</script>


<style></style>