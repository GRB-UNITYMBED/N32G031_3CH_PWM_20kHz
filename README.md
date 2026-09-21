# N32G031_3CH_PWM_20kHz — High-Frequency PWM & System Safety

![pwm_wwdg](doc/images/3CH_PWM_20kHz.png)

An advanced embedded control project designed to demonstrate high-frequency hardware PWM generation and system reliability using the **Nations N32G031** (Cortex-M0). This project highlights how to synchronize multiple timers (TIM1, TIM3) at 20kHz, dynamically adjust duty cycles via an analog potentiometer, and implement a bare-metal Window Watchdog (WWDG) to ensure system stability. This project is fully optimized for cross-platform workflows using UnityMbed.

---

## Wiring

| Device                    | Pin       | N32G031   | Notes                                |
| :------------------------ | :-------- | :-------- | :----------------------------------- |
| **Potentiometer**         | 🧡 Signal | **PA0**   | Analog Input (ADC Channel 0)         |
| (Analog Knob)             | ❤️ VCC    | 3.3V / 5V | Power supply for the knob            |
|                           | 🤎 GND    | GND       | Common system ground                 |
| **Oscilloscope / Driver** | 🔵 Signal | **PA8**   | TIM1 Channel 1 (20kHz PWM Output)    |
| **LED 1 / Output 1**      | 🟢 Signal | **PB0**   | TIM3 Channel 3 (20kHz PWM Output)    |
| **LED 2 / Output 2**      | 🟡 Signal | **PB1**   | TIM3 Channel 4 (20kHz PWM Output)    |
| **Serial Monitor**        | ⚪ TX     | **PA9**   | UART1 TX (115200 baud) for Telemetry |

---

## Behaviour & Execution

| State                    | Actuator / System Response                                                                                                           |
| :----------------------- | :----------------------------------------------------------------------------------------------------------------------------------- |
| **Knob Turned Left**     | ADC reads closer to 0, Duty Cycle approaches 0%. LEDs dim, and oscilloscope shows a narrow pulse.                                    |
| **Knob Turned Right**    | ADC reads closer to 4095, Duty Cycle approaches 100%. LEDs brighten, and oscilloscope shows a wide pulse.                            |
| **Serial Telemetry**     | Prints real-time ADC values and calculated Duty Cycle (%) to the serial monitor every 50,000 loops.                                  |
| **System Safety (WWDG)** | The Window Watchdog is continuously fed. If the main loop hangs or delays unexpectedly, the MCU will automatically reset to recover. |

---

## Hardware Setup & Troubleshooting

- **Debug probe:** Any **CMSIS-DAP** adapter over **SWD** is supported out-of-the-box.
- **Continuous System Resets:** If the MCU keeps resetting, the main loop might be blocked or delayed too long, causing the WWDG to trigger. Ensure `WWDG->CTRL = 0x7F;` is executed within the allowed time window.
- **Garbage Serial Data:** Ensure your Serial Monitor software is set to **115200 baud rate**, 8 Data bits, No Parity, 1 Stop bit (8N1).

---

## Learning & AI Extension Ideas

Students can use the built-in AI Assistant in the IDE to explore, debug, and modify this project. Try pasting these example prompts:

- **To Learn:** `"Explain how the Window Watchdog (WWDG) registers CFG and CTRL are configured in bare-metal C in this code, and why it's safer than a standard Independent Watchdog (IWDG)."`
- **To Experiment:** `"How can I change the PWM frequency from 20kHz to 10kHz by modifying the timer1_period and timer3_period calculations?"`
- **To Debug:** `"My LEDs are flickering instead of dimming smoothly when I turn the knob. Could it be related to the Timer Prescaler or the ADC sampling time?"`

---

## Build and Flash (Universal Cross-Platform)

1. **Open Project:** Open this project folder directly in the IDE.
2. **Build & Flash:** Simply click the **Build** and **Flash** buttons on the interface.

---

Part of the [UnityMbed](https://github.com/GRB-UNITYMBED) N32G031 example set.
