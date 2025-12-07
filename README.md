# Voice-Communication-System-
Mobile-to-mobile communication app using Simulink Mobile enabling real-time encrypted  audio transfer between two phones. 
Captured synchronized gyroscope and accelerometer motion data during voice recording; attached time stamped sensor logs with the encrypted audio stream. 
***
# Features 
Key-Based Frequency Scrambling (configurable shiftKey, multKey).

 Real-time encrypted audio streaming between two Android phones.

 Synchronized motion logging (accelerometer + gyroscope) with audio.

 Live motion plots on receiver device using Time Scope blocks.

 TCP/IP communication over local Wi-Fi network.

 Fully built and deployed using Simulink Support Package for Android Devices.
 ***

 # Sender(Phone-A)
 ## Android Microphone:- 
 Captures real-time audio samples from the phone’s mic.
 ## Pre-processing + FFT :-
 Converts time-domain audio into frequency-domain (freqData) for scrambling.
 ## Scrambling Block
 Inputs: freqData, shiftKey = 247, multKey = 9. 
 Applies key-based permutation on frequency bins to make audio unintelligible. 
 Output is encrypted frequency spectrum. 
 ## IFFT + Quantization
 Converts encrypted spectrum back to time domain and casts to int16, producing audio_i16.
 ## Android Accelerometer + Gyroscope
 Two Android sensor blocks capturing X, Y, Z axes.
 Signals are combined into 3-element vectors and cast to single.
## Packet Formation
### Inputs:

acc_3 → 3-axis accelerometer.

gyo_3 → 3-axis gyroscope.

audio_i16 → encrypted audio stream.

Outputs a synchronized data packet pkt containing motion + audio.

#### TCP/IP Server (Android)

Listens on Port 25000 and sends packets to the receiver over Wi-Fi (local network).![WhatsApp Image 2025-10-10 at 20 47 41_3bca05fd](https://github.com/user-attachments/assets/d129dda0-bba2-4f62-9a0e-43a97fcdfe13)
![WhatsApp Image 2025-12-07 at 11 38 53_895f3e2f](https://github.com/user-attachments/assets/fa8e944b-cf51-4c8b-96db-dac893e4e271)

***
# Receiver (Phone B)
##  TCP/IP Client (Android)

Connects to sender using IP (e.g., 10.7.9.204) and Port 25000.

Receives combined data stream pkt.

## Unpack Block (unpack_audio_motion)

### Separates incoming packet into:

acc_3 (accelerometer 3-axis)

gyo_3 (gyroscope 3-axis)

audio_i16 (encrypted audio samples)

## Motion Visualization

acc_3 and gyo_3 are split into Ax, Ay, Az.

Each axis is connected to Time Scope blocks (Time Scope1–5) to show real-time motion graphs on the phone.

## FFT (computeFFT)

Converts audio_i16 to frequency-domain data for descrambling.
## Key Input (Android UI)

Two Android numeric input blocks (Data input 1, Data input 2) allow user to configure shiftKey and multKey from the app UI.

## Descrambling Block (descramble – MATLAB Function)

Inputs: scrambled freqData, shiftKey, multKey.

Reverses the permutation applied at the sender to recover original spectrum.

## IFFT + int16 + fcn

computeIFFT → returns time-domain audio.

int16 → converts to 16-bit format.

fcn → reshape/scale to match Android audio driver requirements.

## Android Speaker

Plays back the decrypted audio stream.

![WhatsApp Image 2025-10-10 at 20 48 11_30984f1c](https://github.com/user-attachments/assets/0aef18a6-a6ee-4893-bba4-102c640d0ec8)
![WhatsApp Image 2025-10-10 at 21 09 04_0d73baa9](https://github.com/user-attachments/assets/410db5ae-4fc1-4991-8b5b-1bbe5e305ce4)  ![WhatsApp Image 2025-10-10 at 21 09 05_272d759c](https://github.com/user-attachments/assets/7a67613f-375a-4af3-b440-143891f71ac9) ![WhatsApp Image 2025-10-10 at 21 09 05_4c1ed38a](https://github.com/user-attachments/assets/248b528e-bcf6-4f18-871f-c4d06ee7eb2f)




 
