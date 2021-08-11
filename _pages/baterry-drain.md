---
permalink: /battery-drain/
title: "배터리 드레인"
---

<section id="batteryText">
    배터리 체크를 지원하지 않습니다.
</section>
<section id="targetBatteryText">
    배터리 체크를 지원하지 않습니다.
</section>
<section id="wakeLockText">
    wakeLock을 지원하지 않습니다.
</section>

<iframe id="loadGpuBox" src="https://qlqhqo2341.github.io/3-2_computer_graphics/Project3/base.htm">
</iframe>

<script>
    let wakeLock = null;
    let targetBattery = parseInt(location.search.replace("?","")) || 80;

    const wakeLockText = document.getElementById("wakeLockText");
    const batteryText = document.getElementById("batteryText");
    const targetBatteryText = document.getElementById("targetBatteryText");
    
    targetBatteryText.textContent = `목표 배터리 : ${targetBattery}%`;

    const batteryChecker = async (batteryFunc) => {
        
        if (!batteryFunc) {
            return;
        }

        let battery = await batteryFunc();
        let batteryLevel = battery.level;
        const intBatteryLevel = parseInt(batteryLevel * 100);

        console.log(intBatteryLevel);
        batteryText.textContent = `남은 배터리 : ${intBatteryLevel}%`;
        if ((intBatteryLevel <= targetBattery) && wakeLock) {
            const w = wakeLock;
            await wakeLock.release()
            wakeLock = null;

            wakeLockText.textContent = "목표 배터리량이 충족되어 wakeLock을 삭제했습니다.";

            const gpuBox = document.getElementById('loadGpuBox');
            if (gpuBox) {
                gpuBox.remove();
            }
        }

        setTimeout(() => {batteryChecker(batteryFunc)}, 5000);
    };

    let init = async () => {
        if (navigator && navigator.wakeLock) {
            const setWakeLock = async () => {
                wakeLock = await navigator.wakeLock.request('screen');
                wakeLockText.textContent = "wakeLock이 설정 되었습니다.";
            };
            await setWakeLock();

            wakeLock.addEventListener('release', () => {
            // the wake lock has been released
                wakeLockText.textContent = "wakeLock이 해지 되었습니다.";
            });
            document.addEventListener('visibilitychange', async () => {
                if (wakeLock !== null && document.visibilityState === 'visible') {
                    await setWakeLock();
                }
            });
        }

        let batteryFunc;
        if (navigator && navigator.getBattery) {
            batteryFunc = () => { return navigator.getBattery()};
        } else if (navigator && navigator.battery) {
            batteryFunc = () => {return navigator.battery};
        }
        batteryChecker(batteryFunc);
    }; 
    init();
</script>


