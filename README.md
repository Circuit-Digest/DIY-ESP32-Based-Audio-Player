# ESP32 Based Audio Player

![ESP32 Audio Player](https://circuitdigest.com/sites/default/files/projectimage_mic/ESP32-Audio-Player-using-LM386.jpg)

## Project Overview

This project demonstrates how to build a DIY audio player using the ESP32 microcontroller and LM386 amplifier IC. The ESP32's built-in Digital-to-Analog Converter (DAC) is utilised to play audio files in WAV format through a speaker, creating a simple yet functional audio playback system.

**Project Link:** [ESP32 Based Audio Player on CircuitDigest](https://circuitdigest.com/microcontroller-projects/esp32-based-audio-player)

---

## Features

- 🎵 Play audio files using ESP32's DAC capability
- 🔊 Audio amplification using LM386 IC
- 📁 Support for WAV audio format
- 🎚️ Adjustable volume control through potentiometer
- ⚡ Simple circuit with minimal components
- 🔄 Continuous audio playback loop

---

## Components Required

| Component | Quantity | Description |
|-----------|----------|-------------|
| ESP32 Development Board | 1 | Main microcontroller |
| LM386 Amplifier Module | 1 | Audio amplification |
| 8Ω Speaker | 1 | Audio output |
| Jumper Wires | As needed | Circuit connections |
| Breadboard (Optional) | 1 | For prototyping |

---

## Circuit Diagram

### Pin Connections

| LM386 Amplifier Module | ESP32 Pin |
|------------------------|-----------|
| VCC | VIN (5V) |
| GND | GND |
| IN | GPIO 25 (DAC1) |

**Speaker Connection:** Connect the 8Ω speaker to the output pins of the LM386 amplifier module.

### Circuit Notes
- GPIO 25 is one of the two DAC (Digital to Analog Converter) pins on the ESP32
- The LM386 amplifier module amplifies the weak audio signal from the ESP32
- Volume can be adjusted using the potentiometer on the LM386 module

---

## Software Requirements

### Arduino IDE Setup
1. **Arduino IDE** (version 1.8 or higher)
2. **ESP32 Board Support** - Install via Board Manager
3. **XT_DAC_Audio Library** - Download from [XTronical](https://www.xtronical.com/the-dacaudio-library-download-and-installation/)

### Audio Processing Tools
1. **Audacity** - For audio file conversion and processing ([Download here](https://www.audacityteam.org/))
2. **HxD Hex Editor** - For generating hex codes from WAV files ([Download here](https://mh-nexus.de/en/hxd/))

---

## Audio File Preparation

### Step 1: Audio Conversion with Audacity
1. Open your audio file in Audacity
2. Change the **Project Rate** to **16000 Hz** (8000-16000 Hz recommended for ESP32)
3. Navigate to **File > Export > Export as WAV**
4. Select the following settings:
   - **File Type:** WAV (Microsoft)
   - **Encoding:** Unsigned 8-bit PCM
5. Save the file

### Step 2: Generate Hex Code
1. Open the WAV file in HxD Hex Editor
2. Press **Ctrl+A** to select all hex code
3. Go to **Edit > Copy as > C** to copy in C language format
4. Paste the code into a header file named `SoundData.h`

### SoundData.h Format
```cpp
const unsigned int sample[] = {
    // Paste your hex code here
};
```

---

## Code Structure

### Main Libraries
```cpp
#include "SoundData.h"      // Contains audio data in hex format
#include "XT_DAC_Audio.h"   // DAC audio library
```

### Key Objects
```cpp
XT_Wav_Class Sound(sample);        // WAV file object
XT_DAC_Audio_Class DacAudio(25,0); // DAC audio player on GPIO 25
```

### Setup Function
```cpp
void setup() {
    Serial.begin(115200);  // Initialize serial for debugging
}
```

### Loop Function
```cpp
void loop() {
    DacAudio.FillBuffer();              // Fill audio buffer
    if(Sound.Playing==false)            // Check if sound finished
        DacAudio.Play(&Sound);          // Play sound again
}
```

---

## Installation & Usage

### Step 1: Library Installation
1. Download and install the XT_DAC_Audio library
2. Place it in your Arduino libraries folder

### Step 2: Prepare Your Audio
1. Convert your audio file to WAV format (16000 Hz, 8-bit PCM)
2. Generate hex code using HxD Hex Editor
3. Create `SoundData.h` file with the hex data

### Step 3: Upload Code
1. Connect ESP32 to your computer via USB
2. Select the correct board and port in Arduino IDE
3. Upload the sketch to ESP32

### Step 4: Hardware Setup
1. Connect the LM386 amplifier module to ESP32 as per the circuit diagram
2. Attach the speaker to the amplifier output
3. Power up the ESP32

### Step 5: Testing
1. Once powered, the ESP32 will automatically start playing the audio
2. Adjust volume using the potentiometer on the LM386 module
3. Monitor serial output for debugging

---

## Code Implementation

```cpp
#include "SoundData.h"
#include "XT_DAC_Audio.h"

XT_Wav_Class Sound(sample);
XT_DAC_Audio_Class DacAudio(25,0);

uint32_t DemoCounter=0;

void setup() {
    Serial.begin(115200);
}

void loop() {
    DacAudio.FillBuffer();
    if(Sound.Playing==false)
        DacAudio.Play(&Sound);
    Serial.println(DemoCounter++);
}
```

---

## Troubleshooting

### Problem: Only Static Noise
**Solutions:**
- Verify the audio file is in correct format (16000 Hz, 8-bit PCM)
- Check all connections, especially GPIO 25 to amplifier IN pin
- Ensure hex code is properly formatted in SoundData.h
- Verify amplifier module is powered correctly

### Problem: No Sound Output
**Solutions:**
- Check speaker connections
- Verify LM386 module is receiving power (VIN and GND)
- Test speaker with another audio source
- Ensure GPIO 25 is properly connected

### Problem: Low Volume
**Solutions:**
- Adjust potentiometer on LM386 module clockwise
- Use a higher wattage speaker
- Check amplifier module power supply

### Problem: Distorted Audio
**Solutions:**
- Reduce sample rate in Audacity (try 8000 Hz)
- Lower the volume on amplifier module
- Ensure power supply is stable

---

## Technical Specifications

- **Microcontroller:** ESP32
- **DAC Pin Used:** GPIO 25 (DAC1)
- **Audio Format:** WAV (Unsigned 8-bit PCM)
- **Sample Rate:** 8000-16000 Hz (recommended)
- **Amplifier IC:** LM386
- **Speaker Impedance:** 8Ω
- **Power Supply:** 5V via USB or external source

---

## How It Works

1. **Audio Storage:** The audio file is converted to a hex code and stored in program memory as an array
2. **DAC Conversion:** ESP32's DAC converts digital audio data to analog voltage signals on GPIO 25
3. **Amplification:** LM386 amplifies the weak analog signal from ESP32
4. **Audio Output:** The amplified signal drives the speaker to produce audible sound
5. **Continuous Playback:** The code continuously checks if audio has finished and replays it in a loop

---

## Limitations

- **Memory Constraints:** ESP32's limited RAM restricts the length of audio files
- **Audio Quality:** 8-bit, low sample rate audio provides basic quality
- **Format Support:** Only WAV format is supported
- **Mono Output:** Single channel audio only

---

## Future Enhancements

- [ ] Add SD card support for larger audio files
- [ ] Implement multiple track playback
- [ ] Add touch/button controls for play/pause/skip
- [ ] Integrate Bluetooth audio streaming
- [ ] Display track information on OLED screen
- [ ] Add equaliser functionality
- [ ] Battery power option with charging circuit

---

## Resources

- **Project Tutorial:** [CircuitDigest ESP32 Audio Player](https://circuitdigest.com/microcontroller-projects/esp32-based-audio-player)
- **XT_DAC_Audio Library:** [XTronical Website](https://www.xtronical.com/the-dacaudio-library-download-and-installation/)
- **Audacity:** [Official Website](https://www.audacityteam.org/)
- **HxD Hex Editor:** [Official Download](https://mh-nexus.de/en/hxd/)
- **ESP32 Datasheet:** [Espressif Documentation](https://www.espressif.com/en/products/socs/esp32)
- **LM386 Datasheet:** [Texas Instruments](https://www.ti.com/product/LM386).
- **ESP32:** [ESP32 Projects](https://circuitdigest.com/esp32-projects)

## License

This project is open-source and available for educational and personal use. Please take a look at the original CircuitDigest article for specific licensing information.

---

## Contributing

You can fix this project and submit pull requests for improvements. Suggestions and bug reports are welcome!

---

## Acknowledgments

- Circuit Digest for the original project tutorial
- XTronical for the XT_DAC_Audio library
- ESP32 community for continued support and development

---

## Contact & Support

For questions or support regarding this project, please visit the [original project page](https://circuitdigest.com/microcontroller-projects/esp32-based-audio-player) or refer to the CircuitDigest community forums.

---

**Happy Making! 🎵🔧**
