# Physical Layer: Numerical Examples

## Data Rate Calculations

### 1. Nyquist Bit Rate Examples (Noiseless Channel)

The Nyquist formula for maximum data rate in a noiseless channel is:
**BitRate = 2 × Bandwidth × log₂(L)**

Where:
- Bandwidth is in Hz
- L is the number of signal levels
- BitRate is in bits per second (bps)

#### Example 1.1
**Question:** Consider a noiseless channel with a bandwidth of 3000 Hz transmitting a signal with two signal levels. What is the maximum bit rate?

**Solution:**
BitRate = 2 × 3000 × log₂(2)
BitRate = 2 × 3000 × 1
BitRate = 6000 bps or 6 kbps

#### Example 1.2
**Question:** We need to send 265 kbps over a noiseless channel with a bandwidth of 20 kHz. How many signal levels do we need?

**Solution:**
265,000 = 2 × 20,000 × log₂(L)
log₂(L) = 265,000 / (2 × 20,000)
log₂(L) = 6.625
L = 2^6.625 ≈ 99 signal levels

#### Example 1.3
**Question:** A noiseless channel has a bandwidth of 5000 Hz. What is the maximum bit rate if we use 8 signal levels?

**Solution:**
BitRate = 2 × 5000 × log₂(8)
BitRate = 2 × 5000 × 3
BitRate = 30,000 bps or 30 kbps

### 2. Shannon Capacity Examples (Noisy Channel)

The Shannon formula for maximum data rate in a noisy channel is:
**Capacity = Bandwidth × log₂(1 + SNR)**

Where:
- Bandwidth is in Hz
- SNR is the signal-to-noise ratio (linear, not dB)
- Capacity is in bits per second (bps)

For SNR in decibels (dB): SNR(linear) = 10^(SNR(dB)/10)

#### Example 2.1
**Question:** A telephone line has a bandwidth of 3000 Hz with an SNR of 3162. Calculate the channel capacity.

**Solution:**
Capacity = 3000 × log₂(1 + 3162)
Capacity = 3000 × log₂(3163)
Capacity = 3000 × 11.62
Capacity = 34,860 bps or approximately 34.9 kbps

#### Example 2.2
**Question:** The SNR of a channel is 36 dB and the bandwidth is 2 MHz. Calculate the theoretical channel capacity.

**Solution:**
SNR(linear) = 10^(36/10) = 10^3.6 = 3981
Capacity = 2 × 10^6 × log₂(1 + 3981)
Capacity = 2 × 10^6 × log₂(3982)
Capacity = 2 × 10^6 × 11.96
Capacity = 23.92 × 10^6 bps or approximately 24 Mbps

#### Example 2.3
**Question:** What bandwidth is needed to transmit data at a rate of 50 Mbps if the channel has an SNR of 15 dB?

**Solution:**
SNR(linear) = 10^(15/10) = 10^1.5 = 31.62
50 × 10^6 = Bandwidth × log₂(1 + 31.62)
50 × 10^6 = Bandwidth × 5
Bandwidth = 10 × 10^6 Hz = 10 MHz

## Attenuation Calculations

Attenuation in decibels (dB) is calculated as:
**Attenuation (dB) = 10 × log₁₀(P₁/P₂)**

Where:
- P₁ is the power at the sending end
- P₂ is the power at the receiving end

#### Example 3.1
**Question:** If the signal power at the sending end is 100 mW and the power at the receiving end is 0.1 mW, what is the attenuation in dB?

**Solution:**
Attenuation = 10 × log₁₀(100/0.1)
Attenuation = 10 × log₁₀(1000)
Attenuation = 10 × 3
Attenuation = 30 dB

#### Example 3.2
**Question:** A signal experiences 25 dB of attenuation. If the transmitted power is 200 mW, what is the received power?

**Solution:**
25 = 10 × log₁₀(200/P₂)
2.5 = log₁₀(200/P₂)
10^2.5 = 200/P₂
316.23 = 200/P₂
P₂ = 200/316.23 = 0.63 mW

## Propagation Delay Calculations

Propagation delay is calculated as:
**Propagation Delay = Distance / Propagation Speed**

#### Example 4.1
**Question:** The distance between two points is 10,000 km. The propagation speed in the medium is 2 × 10^8 m/s. What is the propagation delay?

**Solution:**
Propagation Delay = 10,000 × 10^3 / (2 × 10^8)
Propagation Delay = 10^7 / (2 × 10^8)
Propagation Delay = 0.05 seconds or 50 milliseconds

#### Example 4.2
**Question:** A signal takes 30 ms to propagate from one point to another. If the propagation speed is 2.5 × 10^8 m/s, what is the distance between the points?

**Solution:**
Distance = Propagation Speed × Propagation Delay
Distance = 2.5 × 10^8 × 30 × 10^-3
Distance = 7.5 × 10^6 meters or 7500 km

## Bandwidth-Delay Product Examples

The bandwidth-delay product represents the maximum number of bits that can be in transit at any time:
**Bandwidth-Delay Product = Bandwidth × Delay**

#### Example 5.1
**Question:** A link has a bandwidth of 1 Mbps and a propagation delay of 20 ms. Calculate the bandwidth-delay product.

**Solution:**
Bandwidth-Delay Product = 10^6 × 20 × 10^-3
Bandwidth-Delay Product = 20,000 bits

This means that to utilize the full capacity of this link, we need to have 20,000 bits in transit at any time.

#### Example 5.2
**Question:** A satellite link has a bandwidth of 10 Mbps and a round-trip time of 500 ms. What is the bandwidth-delay product, and what does this mean for effective data transmission?

**Solution:**
Bandwidth-Delay Product = 10^7 × 500 × 10^-3
Bandwidth-Delay Product = 5 × 10^6 bits or 5 Mbits

This means that to fully utilize this satellite link, the sending device must transmit 5 million bits before receiving any acknowledgment. This is why protocols like TCP may struggle with satellite links unless appropriately configured with large window sizes.

## Multiplexing Examples

#### Example 6.1: Frequency Division Multiplexing
**Question:** A communication channel with a bandwidth of 300 kHz is to be shared by 6 users using FDM. If a guard band of 5 kHz is used between adjacent channels, what is the bandwidth available to each user?

**Solution:**
Total guard band = 5 kHz × 5 (number of guard bands between 6 channels) = 25 kHz
Available bandwidth for users = 300 kHz - 25 kHz = 275 kHz
Bandwidth per user = 275 kHz / 6 = 45.83 kHz

#### Example 6.2: Time Division Multiplexing
**Question:** A TDM system multiplexes 4 sources, each with a bit rate of 2 kbps. What is the minimum required bit rate for the multiplexed line, assuming no overhead?

**Solution:**
Minimum bit rate = Sum of individual bit rates
Minimum bit rate = 4 × 2 kbps = 8 kbps

With framing bits or other overhead, the actual required rate would be higher.
