# ImageToC POV Builder

This project is a browser-based generator that turns an image (or hand-drawn pixel grid) into an Arduino sketch for a rotating RGB POV display.

The generated sketch is configured for:

- **Arduino Nano V3**
- **Common-cathode RGB LEDs**
- **One Hall-effect sensor** for revolution timing

---

## 1) Wiring how-to

Use the wiring image as the visual reference, then follow this checklist.

### A) Core power rails

1. Connect Nano **5V** to the breadboard positive rail.
2. Connect Nano **GND** to the breadboard ground rail.
3. Connect your battery/regulator output so the Nano and LED rails share a **common ground**.

> The diagram shows a 9V battery clip; if you power from 9V, feed the Nano through a suitable input path and keep all grounds common.

### B) RGB bus lines (shared across all LEDs)

The generated code defaults to:

- **RED_PIN = D8**
- **GREEN_PIN = D9**
- **BLUE_PIN = D10**

Wire those three Nano pins to the three color bus lines through current-limiting resistors (as shown in the diagram). These color lines are shared by every LED in the column.

### C) LED commons (one per LED)

For each common-cathode RGB LED:

- Tie each LED color lead to the shared **R/G/B bus**.
- Run the LED **common cathode** to its own Nano pin listed in the app’s **Common pins** field.

Default common pin list from the app:

- `0, 1, A1, 3, 4, 5, 6, 7`

That default means **8 LEDs total** (one common-select pin per LED).

### D) Hall-effect sensor (replace the part labeled in the image)

The diagram callout says to replace that device with a Hall sensor.

Default Hall input pin in generated code:

- **HALL_PIN = D2** (interrupt capable on Nano)

Typical Hall sensor wiring:

- Hall **VCC** → 5V
- Hall **GND** → GND
- Hall **OUT** → D2

Mount a small magnet on the rotating assembly so one pulse occurs per revolution.

### E) LED orientation reminder

From your LED pinout image:

- It is a **common-cathode** part.
- Verify each lead (R, cathode, G, B) before final soldering.

---

## 2) Using the program (index.html)

1. Open `index.html` in a desktop browser.
2. Click **Browse…** and choose an image.
3. Confirm preview and dimensions.
4. Optionally edit pixels directly in the grid (paint/erase/fill).
5. Set pin fields if your wiring differs:
   - Red pin
   - Green pin
   - Blue pin
   - Hall pin
   - Common pins list (comma-separated)
6. Click **Convert / Generate** to build the Arduino sketch.
7. Use **Copy** or **Save As** to export the generated `.ino`/`.h` style code.

### Practical tips

- Keep image width moderate (fewer columns = faster updates).
- Keep height equal to your physical LED count.
- If displayed colors are swapped, re-check RGB lead mapping.

---

## 3) Uploading code to Arduino Nano

1. Install and open **Arduino IDE**.
2. Create a new sketch, then paste the generated code (or open the file you saved).
3. In **Tools → Board**, select **Arduino Nano**.
4. In **Tools → Processor**, choose the correct bootloader variant for your board.
5. In **Tools → Port**, select your Nano serial port.
6. Click **Verify** (compile) and fix any pin conflicts if needed.
7. Click **Upload**.
8. Power the POV assembly and rotate it; position the Hall sensor so pulses are clean and repeatable.

### If it does not render correctly

- No image: check Hall sensor wiring and magnet placement.
- Flicker/jitter: ensure stable power and secure grounds.
- Missing rows: verify each LED common pin matches the app’s common pin list.
- Wrong colors: confirm RGB lead order on your LED package.

---

## 4) Matching software and hardware

Always keep these in sync:

- App pin fields
- Generated code defines
- Physical wire connections

If you rewire later, regenerate code with updated pin values before uploading again.
