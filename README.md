# FPGA Desktop Clock Project

## Overview

This project implements a multi-functional desktop clock on an FPGA, featuring a Python-based GUI for advanced control and monitoring. The clock provides timekeeping (hours, minutes, seconds, day, month), an alarm, and visual feedback through 7-segment displays and RGB LEDs. The system is designed to be highly interactive, allowing users to set the time and alarm both manually on the FPGA board and remotely via the Python application.

## Features

- **Time and Date Display**: The clock displays the current time (HH:MM) and date (Month Day) on an 8-digit 7-segment display.
- **Manual and UART Control**: The time and alarm can be set using the buttons and switches on the FPGA board or through the Python GUI, which communicates with the FPGA over UART.
- **Alarm Functionality**: A configurable alarm can be set, which triggers a visual alert on the RGB LEDs.
- **Python GUI**: A user-friendly desktop application for monitoring the clock, setting the time, and configuring the alarm. It also provides a feature to sync the FPGA clock with the PC's time.
- **Visual Feedback**: Onboard LEDs are used to display the seconds, and RGB LEDs provide a visual indicator for the alarm status.

## Repository Structure

```
.
├── constraints/
│   └── Nexys-4-DDR-Master.xdc  # Constraints file for the Nexys 4 DDR board
├── hdl/
│   ├── RGB_controller.v        # Controls the RGB LEDs for the alarm
│   ├── alarm_control.v         # Manages the alarm logic
│   ├── bcd_unit_counter.v      # A generic BCD counter
│   ├── clock_project_top.v     # The top-level Verilog module
│   ├── day_counter.v           # A specialized counter for days of the month
│   ├── debouncer.v             # A simple button debouncer
│   ├── hex7seg.v               # Drives the 7-segment display
│   ├── hexled.v                # Drives the onboard LEDs
│   ├── rategen.v               # Generates a 1 Hz signal from the 100MHz clock
│   ├── set_control.v           # Manages the logic for setting the time
│   ├── time_core.v             # The core timekeeping module
│   ├── uart_rx.v               # UART receiver module
│   └── uart_tx.v               # UART transmitter module
├── python_app/
│   └── dual_mode_uart.py       # The Python GUI application
└── README.md
```

## Setup and Usage

### Prerequisites

- **Hardware**: A Xilinx FPGA board (the project is configured for a Nexys 4 DDR).
- **Software**:
    - Xilinx Vivado for synthesizing and implementing the Verilog code.
    - Python 3 with the `pyserial` and `tkinter` libraries installed.

### Hardware Setup

1. Clone the repository to your local machine.
2. Open the project in Xilinx Vivado.
3. Add the files from the `hdl` directory as design sources.
4. Add the file from the `constraints` directory as a constraint source.
5. Synthesize and implement the design.
6. Generate the bitstream and program the FPGA board.

### Python Application Setup

1. Connect the FPGA board to your computer via USB (ensure the UART drivers are installed).
2. Determine the COM port assigned to the FPGA (e.g., COM3 on Windows, /dev/ttyUSB0 on Linux).
3. Navigate to the `python_app` directory.
4. Install the required Python libraries:
   ```bash
   pip install pyserial
   ```
5. Open `dual_mode_uart.py` and update the `COM_PORT` variable to match your connected port.
6. Run the application:
   ```bash
   python dual_mode_uart.py
   ```

### Usage

#### On the FPGA Board

- **Setting the Time**:
  1. Press the "set" button (`btn_set` mapped to `BTNC`) to enter time-setting mode.
  2. Use the slide switches (`SW[3:0]`) to set the month, then press "enter" (`BTND`).
  3. Repeat for the day, hour, minute, and second using the slide switches and "enter" button.
  4. The clock will start running with the new time.
- **Setting the Alarm**:
  1. Press the "alarm" button (`btn_alm` mapped to `BTNL`) to enter alarm-setting mode.
  2. Use the slide switches to set the hour, then press "enter".
  3. Repeat for the minute.
- **Visuals**:
  - The 7-segment display shows the date and time.
  - The strip of LEDs displays the current seconds in binary.
  - The RGB LEDs indicate alarm status (Fading = Active, Blinking = Wake/Ringing).

#### Python GUI

- **Monitoring**: The main screen of the application displays the current time and date received from the FPGA over UART.
- **Setting the Time**:
  1. Click "Open Settings Panel".
  2. **Manual Setting**: Enter values for Month, Day, Hour, Minute, and Second, then click "Set Manual Time and Return".
  3. **Auto Sync**: Click "Sync PC Time Now and Return" to instantly send your computer's current time to the FPGA.
- **Setting the Alarm**:
  1. In the settings panel, set the desired alarm hour and minute.
  2. Enable the alarm checkbox.
  3. Click "Set Alarm and Return".

## Documentation

Each source file in this repository is fully documented with comments and docstrings.
- **Verilog Modules**: Header comments describe the module's purpose, inputs, and outputs.
- **Python Code**: Google-style docstrings are provided for the class and all methods, detailing their functionality, arguments, and return values.
