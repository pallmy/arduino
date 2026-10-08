# arduino

james palma softflyers
controller for stepper motor von mises experiment

## Usage
Serial @ **115200**. The sketch boots **idle and disabled** — the motor will
not move until you press `t`. Press `?` for the command list
(`t` run/stop, `h` enable, `k` kill, `d` direction, `i`/`r` impulse/reverse,
`+`/`-` speed, `]`/`[` accel).

## Safety — stopping unwanted spins
Firmware (already handled in `code`):
- Boots **idle + driver disabled**, so it does not spin on power-up, on a
  board reset, or when the laptop sleeps/wakes (USB reset).
- Flushes and ignores stray serial for ~0.8 s after boot, so a sleep/wake
  reconnect can't inject a command.

Hardware — do these too, firmware alone isn't enough:
- **Physical switch on the motor DC power** to the driver. Off = it cannot
  spin, period. Make that the default resting state.
- **10 µF cap RESET→GND** on the Arduino to disable USB auto-reset (remove
  it when you need to upload new code).
- **10 kΩ pull-downs on STEP and DIR** so they don't float during a reset.

## Driver / motor notes
- DM542 supply is **20–50 V**; 20 V is the floor, not the target. Run it
  near **48 V** (under the 50 V max) for high-speed torque and consistency.
  Low voltage = torque collapses at speed = inconsistent high-speed triggers.
- Set the driver current DIP for the motor's rating (~4.0 A for a 23HS32).
- Motor-phase wiring ~**20 AWG** (not thin jumpers — they carry up to 4 A).
- "Commanded steps/s" is not actual RPM; verify with a slo-mo rev count or
  a gyro before trusting high-speed numbers.
