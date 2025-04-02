# Chapter 2: Physical Layer Notes

## 2.1 Functions of Physical Layer

The physical layer is the first and lowest layer in the OSI model, responsible for the physical connection between devices. It provides the hardware means of sending and receiving data.

**Key Functions:**

1. **Representation of Bits:** Converts data bits into electrical, radio, or optical signals
   - Defines encoding methods for converting 0's and 1's into signals
   - Delivers bit-by-bit data to the MAC layer above it

2. **Data Rate (Transmission Rate):** Defines how many bits are transmitted per second
   - Also defines the duration of a bit

3. **Synchronization of Bits:** Ensures clocks of sender and receiver are synchronized
   - Critical for accurate data transmission

4. **Line Configuration:** Manages the physical connection between devices
   - Point-to-point connections through dedicated links

5. **Physical Topology:** Determines how devices are connected in a network
   - Types include mesh, star, ring, and bus topologies

6. **Transmission Mode:** Defines the direction of data flow
   - Simplex: One-way transmission
   - Half-duplex: Two-way transmission but not simultaneous
   - Full-duplex: Two-way simultaneous transmission

**Physical Layer Devices:** Hubs, Repeaters, Modems, Cables

## 2.2 Data and Signals

### Analog vs Digital Signals

| Analog Signals | Digital Signals |
|----------------|-----------------|
| Continuous signals | Discrete signals |
| Represented by sine waves | Represented by square waves |
| Examples: Human voice, natural sounds | Examples: Computer data, binary information |
| Continuous range of values | Discontinuous values |
| Records sound waves as they are | Converts into binary waveform |
| Used in analog devices | Used in digital electronics |

![analog and digital](/images/Screenshot%202025-03-27%20at%2018.24.53.png)

**Signal Types:**
- **Periodic Signal:** Completes a pattern within a measurable timeframe and repeats
- **Non-periodic Signal:** Changes without exhibiting a repeating pattern
- **Period (T):** Time needed to complete one cycle (measured in seconds)
- **Frequency (f):** Number of cycles per second (Hz)
  - Formula: f = 1/T and T = 1/f

### Transmission Impairments

Factors that degrade signal quality during transmission:

1. **Attenuation:** Loss of signal energy over distance
   - Measured in decibels (dB)
   - Requires amplifiers to compensate

![Attenuation](/images/Screenshot%202025-03-27%20at%2018.25.21.png)


2. **Distortion:** Changes in signal form or shape
   - Different frequencies travel at different speeds
   - Components of composite signals arrive at different times

![Distortion](/images/Screenshot%202025-03-27%20at%2018.25.44.png)

3. **Noise:** Unwanted signals that mix with the original signal
   - **Thermal Noise:** Random electron movement in wires
   - **Induced Noise:** From motors and appliances
   - **Crosstalk:** When one wire affects another
   - **Impulse Noise:** High-energy bursts from lightning or power lines
   - SNR (Signal-to-Noise Ratio) = Average Signal Power / Average Noise Power

![Noise](/images/Screenshot%202025-03-27%20at%2018.25.58.png)


### Data Rate Limits

The maximum speed of data transmission depends on:
1. Available bandwidth
2. Number of signal levels in digital signal
3. Channel quality (noise level)

**Two theoretical formulas for calculating data rate:**

1. **Noiseless Channel: Nyquist Bit Rate**
   - BitRate = 2 × Bandwidth × log₂(L)
   - Where L is the number of signal levels

2. **Noisy Channel: Shannon Capacity**
   - Capacity = Bandwidth × log₂(1 + SNR)
   - SNR in dB = 10 × log₁₀(SNR)

### Performance Metrics

1. **Bandwidth:** 
   - For analog signals: Range of frequencies (in Hz)
   - For digital signals: Maximum data rate (in bps)

![Noise](/images/Screenshot%202025-03-27%20at%2018.26.15.png)


2. **Throughput:** Actual rate of data transfer (vs. bandwidth as potential)

3. **Latency (Delay):** Time for complete message transmission
   - Propagation time: Time for a bit to travel from source to destination
   - Processing delay: Time for routers to process packet headers
   - Queuing time: Time in intermediate device queues

4. **Bandwidth-Delay Product:** Important metric in data communications
   - Maximum number of bits that can be in transit at any time
   - Formula: Bandwidth × Delay

Case 1:
![Noise](/images/Screenshot%202025-03-27%20at%2018.26.52.png)

Case 2:
![Noise](/images/Screenshot%202025-03-27%20at%2018.27.06.png)

5. **Jitter:** Variation in delay between packets
   - Critical for time-sensitive applications (audio/video)

## 2.3 Data Transmission Media

Transmission media are the physical paths through which data travels from sender to receiver.

![Noise](/images/Screenshot%202025-03-27%20at%2018.27.34.png)


### Guided Media (Wired/Bounded)

Physical links that direct and confine signals in narrow pathways.

1. **Twisted Pair Cable**
   - **Unshielded Twisted Pair (UTP):**
     - Least expensive, easy to install
     - Susceptible to interference
     - Used for telephone applications
   - **Shielded Twisted Pair (STP):**
     - Better performance at higher data rates
     - Eliminates crosstalk
     - More expensive and bulky

2. **Coaxial Cable**
   - Two parallel conductors with separate insulation
   - Modes: Baseband (dedicated) and Broadband (split ranges)
   - Used in cable TV and analog television networks
   - Advantages: High bandwidth, better noise immunity
   - Disadvantage: Single cable failure can disrupt entire network

3. **Optical Fiber Cable**
   - Uses light reflection through glass/plastic core surrounded by cladding
   - Advantages: High capacity, lightweight, low signal attenuation
   - Disadvantages: Difficult to install, expensive, fragile
   - Unidirectional (requires separate fiber for bidirectional communication)

### Unguided Media (Wireless/Unbounded)

No physical medium required for electromagnetic signal transmission.

1. **Radio Waves**
   - Frequency range: 3 KHz - 1 GHz
   - Can penetrate buildings, no antenna alignment needed
   - Used in AM/FM radios, cordless phones
   - Types: Terrestrial and Satellite

2. **Microwaves**
   - Frequency range: 1 GHz - 300 GHz
   - Line-of-sight transmission requiring aligned antennas
   - Used for mobile communications and TV distribution

3. **Infrared**
   - Frequency range: 300 GHz - 400 THz
   - Very short distance, cannot penetrate obstacles
   - Used in TV remotes, wireless peripherals

### Satellites

Communication satellites serve as microwave repeater stations in space.

**Key Concepts:**
- **Uplink/Downlink Frequencies:** Frequencies for ground-to-satellite and satellite-to-ground communication
- **Footprint:** Area receiving useful signal strength from the satellite
- **Frequency Bands:** Common bands are C-band, Ku-band, and Ka-band

**Types of Orbits:**
- Geo-synchronous Earth Orbit
- Geo-stationary Earth Orbit
- Medium Earth Orbit
- Low Earth Orbit

**Satellite Service Types:**
- FSS (Fixed Satellite Services)
- BSS (Broadcast Satellite Services)
- MSS (Mobile Satellite Services)

**Network Segments:**
1. Space Segment: Satellite design and orbit
2. Control Segment: Frequency spectrum and signaling techniques
3. Ground Segment: Earth stations and access techniques

**Advantages:**
- Access to remote areas
- Coverage of large geographical areas
- Insensitivity to terrain
- Distance-insensitive costs
- High bandwidth

**Disadvantages:**
- High initial cost
- Propagation delay (especially with GEO systems)
- Environmental interference
- Regulatory constraints

## 2.4 Bandwidth Utilization

### Multiplexing

Multiplexing allows simultaneous transmission of multiple signals across a single data link, maximizing bandwidth usage.
![Noise](/images/Screenshot%202025-03-27%20at%2018.28.09.png)

**Types of Multiplexing:**

1. **Frequency-Division Multiplexing (FDM)**
   - Divides the frequency range into bands
   - Each user gets a separate frequency band
   - Guard bands prevent interference
   - Used in radio and traditional television broadcasting

![Noise](/images/Screenshot%202025-03-27%20at%2018.28.29.png)

2. **Time-Division Multiplexing (TDM)**
   - Divides time into slots
   - Each user gets the entire bandwidth for a short time slot
   - Two categories:
     - Synchronous TDM: Fixed time slots
     - Statistical/Asynchronous TDM: Dynamic allocation based on need

![Noise](/images/Screenshot%202025-03-27%20at%2018.28.44.png)

3. **Wavelength-Division Multiplexing (WDM)**
   - Similar to FDM but for optical signals
   - Multiple light sources combined into one single light
   - Uses prisms to combine/split light frequencies
   - Used in fiber-optic communications

![Noise](/images/Screenshot%202025-03-27%20at%2018.28.53.png)

### Spread Spectrum

Techniques that deliberately spread a signal in the frequency domain to increase security and resistance to interference.

![Noise](/images/Screenshot%202025-03-27%20at%2018.29.05.png)

**Key Characteristics:**
- Expands bandwidth beyond what's needed (Bss >> B)
- The spreading process is independent of the original signal
- Provides secure transmission and resistance to jamming

**Types:**

1. **Frequency Hopping Spread Spectrum (FHSS)**
   - Uses multiple carrier frequencies switched in a pseudorandom pattern
   - Each transmission hops between frequencies
   - Provides security and allows bandwidth sharing among multiple users

![Noise](/images/Screenshot%202025-03-27%20at%2018.29.18.png)

For E xample M is 8 and k is 3. The pseudorandom code generator will create eight different 3-bit patterns.
These are mapped to eight different frequencies in the frequency table as shown in the following figure.
![Noise](/images/Screenshot%202025-03-27%20at%2018.29.30.png)

2. **Direct Sequence Spread Spectrum (DSSS)**
   - Replaces each data bit with n bits (chips) using a spreading code
   - Chip rate is n times that of the data bit
   - Expands bandwidth while providing security

![Noise](/images/Screenshot%202025-03-27%20at%2018.29.44.png)

In the figure, the spreading code is 11 chips having the pattern 10110111000 (in this case). If the original signal
rate is N, the rate of the spread signal is 11N. This means that the required bandwidth for the spread signal is 11
times larger than the bandwidth of the original signal.
![Noise](/images/Screenshot%202025-03-27%20at%2018.29.59.png)

## 2.5 Switching

Switching is the process of forwarding data from one port to another toward its destination.

**Categories of Switching:**
![Noise](/images/Screenshot%202025-03-27%20at%2018.30.13.png)

1. **Connectionless:** No prior handshaking required; data forwarded based on tables
2. **Connection-Oriented:** Pre-established circuit required before data transmission

### Circuit Switching
![Noise](/images/Screenshot%202025-03-27%20at%2018.30.22.png)

- Dedicated communication path established between nodes
- Three phases: circuit establishment, data transfer, circuit disconnect
- Example: Traditional telephone system
- **Advantages:** Dedicated bandwidth, predictable performance
- **Disadvantages:** Inefficient resource use, long connection setup time

### Message Switching
![Noise](/images/Screenshot%202025-03-27%20at%2018.30.38.png)

- Messages transferred as complete units through intermediate nodes
- Store-and-forward technique (each node stores entire message before forwarding)
- No dedicated path established
- **Advantages:** Reduces congestion, shared channels, priority assignment
- **Disadvantages:** Not suitable for real-time applications, requires large storage capacity

### Packet Switching
![Noise](/images/Screenshot%202025-03-27%20at%2018.30.59.png)

- Message divided into smaller packets sent individually
- Each packet contains header with addressing and sequencing information
- Packets reassembled at destination
- **Approaches:**
  - **Datagram Packet Switching:** Connectionless, each packet routed independently
  - **Virtual Circuit Switching:** Connection-oriented, preplanned route established
- **Advantages:** Cost-effective, reliable, efficient
- **Disadvantages:** Complex protocols, possible delays, potential packet loss
![Noise](/images/Screenshot%202025-03-27%20at%2018.31.11.png)

## 2.6 Telephone, Mobile and Cable Networks for Data Communication

### Telephone Networks

- Originally analog systems (POTS - Plain Old Telephone System)
![Noise](/images/Screenshot%202025-03-27%20at%2018.31.22.png)
The tasks of data transfer and signaling are separated in modern telephone network: data transfer is done by
one network, signaling by another.
![Noise](/images/Screenshot%202025-03-27%20at%2018.31.37.png)

- Modern networks separate data transfer and signaling
- Modems (modulator/demodulator) convert digital data to analog signals and vice versa
![Noise](/images/Screenshot%202025-03-27%20at%2018.32.07.png)
![Noise](/images/Screenshot%202025-03-27%20at%2018.32.19.png)

- DSL (Digital Subscriber Line) technology provides high-speed digital communication over existing local loops
- ADSL (Asymmetric DSL) designed for residential users with faster download than upload speeds
![Noise](/images/Screenshot%202025-03-27%20at%2018.32.31.png)


### Cable Networks

- Originally developed for video services, now used for Internet access
- Also known as Community Antenna Television (CATV)
- **Types:**
  1. **Traditional Cable Networks:** Unidirectional communication
![Noise](/images/Screenshot%202025-03-27%20at%2018.32.51.png)
  2. **Hybrid Fiber-Coaxial (HFC) Networks:** Bidirectional communication
![Noise](/images/Screenshot%202025-03-27%20at%2018.33.09.png)
![Noise](/images/Screenshot%202025-03-27%20at%2018.33.31.png)
![Noise](/images/Screenshot%202025-03-27%20at%2018.33.40.png)
![Noise](/images/Screenshot%202025-03-27%20at%2018.33.52.png)
![Noise](/images/Screenshot%202025-03-27%20at%2018.34.05.png)

### Mobile Networks

Mobile data uses wireless data communications via radio waves.

**Benefits:**
- Remote task execution
- Communication from any location
- Access to office data when mobile
- Electronic audit trail of communications

**Technologies:**

1. **GSM (Global System for Mobile Communications)**
   - Uses TDMA (Time Division Multiple Access)
   - Supports encryption for security
   - Frequency: 900 MHz to 1800 MHz internationally

2. **CDMA (Code Division Multiple Access)**
   - Originally used by British military
   - High service quality

3. **WLL (Wireless in Local Loop)**
   - Wireless local telephone service for homes/offices

4. **GPRS (General Packet Radio Services)**
   - Packet-based wireless communication
   - Charges based on data volume, not time
   - Used in 2G and 3G mobile networks
   - Speed: 56 kbps to 114 kbps (theoretical)
