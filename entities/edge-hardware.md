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

## Pricing pressure, and a proposed second SKU (2026-08-20, not built)

Raised in [[homesteader-labs-site]]: the Pi 5 handheld's BOM has climbed to **~$450** on rising
Raspberry Pi 5 prices, which pushes retail toward ~$700 and, in Alex's read, into "a slice of a
slice" of the addressable market.

**Direction under consideration:** a two-SKU split rather than one hardware product.
- **Free/low-cost app**, [[model-registry]]'s stack ported to **Core ML on Apple Silicon**
  (iPhone Neural Engine) as the acquisition tier and the wider top of the data-collection funnel.
  Inference stays fully on-device (no network call), so the existing offline / "aid not oracle"
  safety design is not compromised by the move, that precondition was about the *inference*
  being local, not the *chassis*.
  - **Distribution risk instead of BOM risk:** trades a manufacturing/BOM problem for an Apple
    App Store gatekeeper problem, 30% cut, iOS-only to start (~30% of phone market), Guideline
    1.4.1 review for a physical-harm/health-adjacent app.
  - **Guideline research (2026-08-20):** 1.4.1 does not ban this category, three comparable apps
    ship today (Picture Mushroom, Seek/iNaturalist, PlantNet). The bar is disclosed methodology +
    validated accuracy claims + "tell the user to consult a professional", which is already what
    the refuse-by-default design does. The harder finding: Public Citizen (citing a controlled
    study) found the best competing app 49% accurate and misidentifying toxic species as safe 44%
    of the time; this is a live, publicly-documented category-wide accuracy scandal, not a
    hypothetical risk. Reframed as validation of the abstain-over-bluff wedge
    ([[anti-slop-principle]]) rather than a reason to avoid the category, provided marketing
    actively distances from "another AI mushroom app."
  - **Recommended next step, not yet started:** ship a minimal TestFlight build with real safety
    copy and submit for review, cheaper than reasoning further from guideline text. Separately,
    the existing Stump-the-Machine upload/consent/dataset flow (built for
    [[forager-field-station]]) would need Apple's Privacy Nutrition Label / ATT disclosure once
    it moves into a native app, since photos + a user identity feed a public CC-BY-4.0 dataset.
  - **What ports vs. what's new:** the hard design work (consent flow, gating logic, dataset
    schema, refuse-by-default router) already exists and is proven on the Space. New work is the
    Core ML conversion of the router + expert models and a native (or webview-hybrid) rebuild of
    the upload UI. Not a green-field build.
- **Pi 5 handheld becomes the premium/"sovereign" SKU**: no app store, no account, no update
  cadence, the actual offline-hardware-ownership tier, positioned as a coherent barbell against
  the free app rather than "a worse container for the same thing."

**Status: direction only, nothing built.** No Core ML port has started, no TestFlight submission
exists, BOM/pricing has not been re-quoted. Revisit this section once either half ships.
