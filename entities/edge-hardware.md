# Entity: edge hardware

The physical field device — the object that ties [[forager-ml]] (the model that runs on it) to
[[homesteader-labs-site]] (the product that sells it).

## The Pi 5 field device (`forager-dev`)
- **Board:** Raspberry Pi 5 · hostname `forager-dev` · `192.168.4.73`
- **NPU:** Hailo 8L M.2 HAT — runs the compiled `.hef` models from [[forager-ml]] (DFC compile).
- **Camera:** Pi Camera Module 3 (IMX708) — must use **CAM0** (the port nearer the USB-C jack);
  CAM1 is not detected.
- **Display:** Waveshare 3.7" eInk HAT · 480×280 · 4-gray (epd3in7 driver).
- **Audio:** USB mic + speaker · Whisper `tiny.en` + pyttsx3/espeak TTS (on-device voice trigger).

## The product
- Sold on [[homesteader-labs-site]] as **WALKING MAN PRO** (catalog: `lib/products.ts`; also
  HELTEC V3). The handheld *is* the productized form of the [[forager-ml]] stack.

## Why it matters here
The model ([[model-registry]]) and the product are two ends of one object. A change to the model
stack (e.g. dropping/adding an expert, or which router ships) has a *product* consequence on the
site, and vice-versa. Keep this edge in mind when either side changes.
