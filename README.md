STM-Learningssss — STM32 Architecture, Firmware, Board SCH, Compiling, etc.
<br>

This repository documents essential concepts and workflow for working with STM32 microcontrollers using STM32CubeIDE.
It covers architecture, firmware, project structure, and how your code ties into CubeMX configuration.

<br><hr><br>

📁 Project Structure
MyProject/
├── Core/
│   ├── Inc/
│   │   └── main.h
│   ├── Src/
│   │   └── main.c
│   ├── Startup/
│   │   └── startup_stm32xxxx.s
│   └── system_stm32xxxx.c
│
├── Drivers/
│   ├── CMSIS/
│   └── STM32xxxx_HAL_Driver/
│
├── Middlewares/
│   └── ...
│
├── STM32CubeMX.ioc
├── .project
├── .cproject
├── Makefile
└── README.md

<br>

In STM32CubeIDE, the files you normally work with are:

<tt>Core/Inc/main.h</tt>

<tt>Core/Src/main.c</tt>

<tt>STM32CubeMX.ioc</tt>

Everything else behaves politely in the background.

<br><hr><br>

1) File Header in <tt>main.c</tt>

At the top, you will notice:

#include "main.h"


This pulls in function declarations, pin configurations, and MCU definitions.
Nothing thrilling — and usually nothing you need to modify.

<br><hr><br>

2) Global Variable Declarations

Typical global handles and buffers look like this:

ADC_HandleTypeDef hadc1;
TIM_HandleTypeDef htim1;

uint16_t adcValues[5];
uint16_t servoSpeedLeft;
uint16_t servoSpeedRight;


These are defined before <tt>main()</tt>, usually inside:

/* USER CODE BEGIN 0 */
/* USER CODE END 0 */


<br><hr><br>

3) Function Prototypes

CubeMX generates peripheral initialisers such as:

void SystemClock_Config(void);
static void MX_GPIO_Init(void);
static void MX_ADC1_Init(void);
static void MX_TIM1_Init(void);


These configure the MCU clocks, GPIOs, ADC, timers, and any other peripherals you've enabled.

<br><hr><br>

4) The <tt>main()</tt> Function — Where Everything Begins
int main(void)
{
    HAL_Init();                   // Initialise the HAL
    SystemClock_Config();         // Set up system clock
    MX_GPIO_Init();               // Configure GPIO
    MX_ADC1_Init();               // ADC initialisation
    MX_TIM1_Init();               // Timer/PWM initialisation

    HAL_TIM_PWM_Start(&htim1, TIM_CHANNEL_1);
    HAL_ADC_Start_DMA(&hadc1, (uint32_t*)adcValues, 5);

    while (1)
    {
        // Application logic lives here
    }
}


Everything critical happens here — initialisation up front, endless logic in the loop.
A bit like life, really.

<br><hr><br>

5) <tt>while(1)</tt> — The Infinite Loop

Anything placed here executes forever:

Reading sensors

Controlling motors

Making decisions

Sending data

Repeating your mistakes until you fix them

<br><hr><br>

6) User Code Sections

CubeMX protects your custom code using blocks like:

/* USER CODE BEGIN 2 */
/* USER CODE END 2 */


Write inside these sections.
Regenerate the project, and your code survives like a stubborn champion.

<br><hr><br>

7) Summary

Declare globals in USER CODE BEGIN 0

All initialisation runs at the start of <tt>main()</tt>

Infinite logic belongs in <tt>while(1)</tt>

Never write outside user-code blocks unless you enjoy chaos

CubeMX handles all boilerplate setup

You focus on actual behaviour and logic

<br><hr><br>

🧩 The <tt>.ioc</tt> File — The Heart of CubeMX

The <tt>STM32CubeMX.ioc</tt> file defines:

MCU selection

Clock tree

Peripheral configuration

GPIO mapping

Middleware options

Code generation behaviour

Opening it in STM32CubeIDE launches the visual CubeMX configurator.

<br> <center> <img width="186" height="273" alt="image" src="https://github.com/user-attachments/assets/c999ba40-8ab9-4b9f-9613-7b977210cc31"/> </center> <br>

Once you’re done tweaking clocks, pins, and peripherals:

→ Click “Generate Code”
CubeMX regenerates initialisation functions accordingly.

<br><hr><br>
