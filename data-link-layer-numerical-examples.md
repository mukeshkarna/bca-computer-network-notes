# Data Link Layer Numericals

Here's a collection of the key numerical problems and examples from Chapter 3 on the Data Link Layer:

## Parity Check Examples

### Example 1: Even Parity
**Problem**: Consider the data unit to be transmitted is 1001001 and even parity is used.

**Solution**:
1. Count the number of 1's in the data unit = 3
2. Since even parity is used and the count is odd, add parity bit = 1
3. Transmitted code word = 10010011
4. If receiver receives 10010011:
   - Count of 1's = 4 (even)
   - No error detected

### Example 2: Detecting Errors
**Problem**: If a single bit in 10010011 is changed during transmission, would even parity detect it?

**Solution**:
1. If any single bit changes, the number of 1's becomes odd
2. Even parity expects an even number of 1's
3. Error would be detected
4. However, if two bits change, the parity would remain even and the error would not be detected

## Checksum Examples

### Example 1: Computing Checksum
**Problem**: Compute the checksum for the data 11001100, 10101010, 11110000, and 11000011 using 8-bit segments.

**Solution**:
1. Add the segments using 1's complement arithmetic:
   - 11001100 + 10101010 = 101110110
   - Carry wraparound: 01110110 + 1 = 01110111
   - 01110111 + 11110000 = 101100111
   - Carry wraparound: 01100111 + 1 = 01101000
   - 01101000 + 11000011 = 100101011
   - Carry wraparound: 00101011 + 1 = 00101100
2. Checksum (1's complement): 11010011
3. Transmitted data: original data + 11010011

### Example 2: Verifying Checksum
**Problem**: Verify if the received data from Example 1 has any errors.

**Solution**:
1. Add all segments including checksum:
   - 11001100 + 10101010 + 11110000 + 11000011 + 11010011 = 1111111111
   - With carry wraparound: 11111111
2. Complement: 00000000
3. Since result is all zeros, no error is detected

## CRC (Cyclic Redundancy Check) Examples

### Example 1: CRC Calculation
**Problem**: A bit stream 1101011011 is transmitted using the standard CRC method. The generator polynomial is x⁴ + x + 1. What is the actual bit string transmitted?

**Solution**:
1. Generator polynomial G(x) = x⁴ + x + 1 is represented as 10011
2. Since G(x) has 5 bits, append 4 zeros to the bit stream: 11010110110000
3. Perform binary division:
   ```
   1101011011000|10011
   10011        |
   -------
   01001
   00000
   -------
   01001
   00000
   -------
   01001
   00000
   -------
   01001
   00000
   -------
   01001
   00000
   -------
   01001
   00000
   -------
   01001
    0011
   -------
     1110
   ```
4. CRC remainder = 1110
5. Replace appended zeros with CRC: 11010110111110
6. This is the transmitted code word

### Example 2: Error Detection with CRC
**Problem**: A bit stream 10011101 is transmitted using the standard CRC method. The generator polynomial is x³ + 1 (1001).
1. What is the actual bit string transmitted?
2. If the third bit from the left is inverted during transmission, how will the receiver detect this error?

**Solution**:
Part 1:
1. Generator polynomial G(x) = x³ + 1 is represented as 1001
2. Append 3 zeros to the bit stream: 10011101000
3. Perform binary division:
   ```
   10011101000|1001
   1001      |
   ----
   0001
   0000
   ----
   0011
   0000
   ----
   0111
   0000
   ----
   1110
   1001
   ----
   1111
   1001
   ----
   1100
   1001
   ----
    100
   ```
4. CRC remainder = 100
5. Replace appended zeros with CRC: 10011101100
6. This is the transmitted code word

Part 2:
1. Third bit from left is inverted: 10111101100
2. The receiver divides this by the generator polynomial:
   ```
   10111101100|1001
   1001      |
   ----
   0101
   0000
   ----
   1011
   1001
   ----
   0101
   0000
   ----
   1011
   1001
   ----
   0101
   0000
   ----
   1011
   1001
   ----
   0100
   ```
3. Remainder = 100 (non-zero)
4. Since remainder is non-zero, error is detected

### Example 3: Simple CRC
**Problem**: Compute the CRC for data 11100 with divisor 1001.

**Solution**:
1. Append 3 zeros to data: 11100000
2. Divide by 1001:
   ```
   11100000|1001
   1001   |
   ----
   1110
   1001
   ----
   1110
   1001
   ----
   1110
   1001
   ----
    111
   ```
3. CRC remainder = 111
4. Replace appended zeros with CRC: 11100111
5. This is the transmitted code word

## Hamming Code Examples

### Example: 7-bit Data with Hamming Code
**Problem**: Find the Hamming code for 7-bit data 1011001.

**Solution**:
1. Data bits: 1011001
2. We need 4 parity bits (positions 1, 2, 4, 8) since 2³ < 7+4+1 ≤ 2⁴
3. Arrange data bits at positions 3, 5, 6, 7, 9, 10, 11: _ _ 1 _ 0 1 1 _ 0 0 1
4. Calculate parity bits:
   - P1 (position 1) checks bits 1, 3, 5, 7, 9, 11: 1+0+1+0+1 = 3 (odd)
     For even parity, P1 = 1
   - P2 (position 2) checks bits 2, 3, 6, 7, 10, 11: 1+1+1+0+1 = 4 (even)
     For even parity, P2 = 0
   - P4 (position 4) checks bits 4, 5, 6, 7: 0+1+1 = 2 (even)
     For even parity, P4 = 0
   - P8 (position 8) checks bits 8, 9, 10, 11: 0+0+1 = 1 (odd)
     For even parity, P8 = 1
5. Complete Hamming code: 1011011001

### Example: Error Detection and Correction
**Problem**: If bit 6 of the previous Hamming code changes from 1 to 0 during transmission, show how the error is detected and corrected.

**Solution**:
1. Received code: 1010011001 (bit 6 changed from 1 to 0)
2. Check parity bits:
   - P1 checks bits 1, 3, 5, 7, 9, 11: 1+1+0+1+0+1 = 4 (even)
     Expected even parity, received even parity → P1 is correct
   - P2 checks bits 2, 3, 6, 7, 10, 11: 0+1+0+1+0+1 = 3 (odd)
     Expected even parity, received odd parity → P2 is incorrect
   - P4 checks bits 4, 5, 6, 7: 0+0+0+1 = 1 (odd)
     Expected even parity, received odd parity → P4 is incorrect
   - P8 checks bits 8, 9, 10, 11: 1+0+0+1 = 2 (even)
     Expected even parity, received even parity → P8 is correct
3. Error position: P4 + P2 = 0100 + 0010 = 0110 (binary) = 6 (decimal)
4. Bit 6 is in error; flip it from 0 to 1 to correct: 1011011001

## ALOHA Efficiency Calculations

### Pure ALOHA
**Formula**: S(pure) = G × e^(-2G)

Where:
- S = throughput (successful frames per frame time)
- G = number of transmission attempts per frame time

**Maximum Efficiency**:
- Occurs when G = 0.5
- S(max) = 0.5 × e^(-1) = 0.5 × 0.368 = 0.184 (18.4%)

### Slotted ALOHA
**Formula**: S(slotted) = G × e^(-G)

**Maximum Efficiency**:
- Occurs when G = 1
- S(max) = 1 × e^(-1) = 0.368 (36.8%)

## CSMA Performance

### CSMA/CD
The maximum efficiency depends on the persistence method and the ratio of propagation time to transmission time:

For 1-persistent CSMA/CD:
- Throughput approaches 50% as load increases when G = 1
- For non-persistent CSMA/CD, throughput can reach up to 90%

## Flow Control Protocol Examples

### Stop-and-Wait Efficiency
**Problem**: Calculate the efficiency of Stop-and-Wait protocol if transmission time is 10 ms and propagation delay is 5 ms.

**Solution**:
1. Time to send one frame = Transmission time + 2 × Propagation delay + ACK transmission time
2. Assuming negligible ACK transmission time:
   - Total time = 10 ms + 2 × 5 ms = 20 ms
3. Efficiency = Transmission time / Total time = 10 ms / 20 ms = 0.5 (50%)

### Go-Back-N Window Size
**Problem**: If the sequence number field is 4 bits, what is the maximum window size for Go-Back-N ARQ?

**Solution**:
1. With 4-bit sequence numbers, we have 2⁴ = 16 possible sequence numbers
2. Maximum window size for Go-Back-N = 2⁴ - 1 = 15 frames

### Selective Repeat Window Size
**Problem**: If the sequence number field is 4 bits, what is the maximum window size for Selective Repeat ARQ?

**Solution**:
1. With 4-bit sequence numbers, we have 2⁴ = 16 possible sequence numbers
2. Maximum window size for Selective Repeat = 2⁴/2 = 8 frames

## Polling Efficiency

**Formula**: Efficiency = T_t / (T_t + T_poll)

Where:
- T_t = transmission time
- T_poll = polling overhead time

**Example**:
If T_t = 100 ms and T_poll = 10 ms, efficiency = 100 / (100 + 10) = 0.91 (91%)

## Token Ring Performance

**Formulas**:
- For a < 1: S = 1/(1 + a/N)
- For a > 1: S = 1/{a(1 + 1/N)}

Where:
- S = throughput
- N = number of stations
- a = T_p/T_t (ratio of propagation delay to transmission time)

**Example**:
For 10 stations with a = 0.1:
S = 1/(1 + 0.1/10) = 1/1.01 = 0.99 (99%)

For 10 stations with a = 2:
S = 1/{2(1 + 1/10)} = 1/2.2 = 0.455 (45.5%)

These numerical examples demonstrate the key concepts and calculations related to the Data Link Layer, providing practical insight into error detection, correction, channel access methods, and protocol efficiency.