# UART_VP

## Verification Plan

| Item | Verification objective | Stimulus / condition | Check method | Expected result |
|---|---|---|---|---|
| **UART_VP_001** | **Verify `rx_data` matching with `o_data`.** Confirm that each byte sent to the UART receiver is reproduced correctly at the DUT data output. | Send legal UART frames containing representative and randomized 8-bit `rx_data` values. Each frame consists of one start bit, eight data bits sent LSB first, and one stop bit. | Store the transmitted `rx_data` as the reference transaction. When `o_valid` indicates that an output byte is available, sample `o_data` and compare it with the corresponding reference value. Perform exactly one comparison for every received frame. | For every legal UART frame, `o_data == rx_data`. No mismatch, missing comparison, or extra comparison is reported. |
| **UART_VP_002** | **Verify `o_valid` is stable.** Confirm that `o_valid` has no unintended transition, glitch, or unknown value. | Exercise both idle periods and legal UART receptions while observing `o_valid` on every rising edge of `clk`. | Use a clock-based monitor or assertion to check that `o_valid` is always a known `0` or `1`, remains stable whenever no valid indication is expected, and changes only at the intended valid-indication boundaries after a complete UART frame. | `o_valid` never becomes `X` or `Z` and has no unintended pulse or transition. It stays inactive when no output byte is available and provides a clean valid indication for each completed legal frame. |

## Pass Criteria

The UART verification plan passes only when both verification items complete without an unexpected error.
