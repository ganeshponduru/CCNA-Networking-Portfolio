# Physical-Link Troubleshooting Checklist

I'm learning not to assume that an interface-down problem is automatically caused by a bad cable. The physical path can contain multiple components, so I need to isolate the fault step by step.

## 1. Confirm the symptom

- Which device, interface, and remote endpoint are affected?
- Is the interface administratively shut down, physically down, or up without expected traffic?
- Did the issue begin after a change, maintenance, cable move, or hardware replacement?
- Is the problem isolated to one link or affecting several links?

## 2. Check both ends

Confirm the local and remote interfaces, their administrative/operational state, configured speed/duplex where applicable, and any reported errors or alarms. Do not assume the remote side is configured correctly just because the local port looks normal.

Example Cisco commands to investigate (availability depends on platform):

```text
show interfaces status
show interfaces
show ip interface brief
show logging
show inventory
show version
```

For optical links, use the platform-supported transceiver diagnostics command to check Tx/Rx optical power, temperature, voltage, and alarms when available.

## 3. Check the physical path

1. Verify the correct transceiver is installed and supported.
2. Check that the module is seated correctly, following safe handling procedures.
3. Check the patch lead and connector type.
4. Inspect and clean fiber connectors using approved tools and procedures; never look directly into a fiber or optical transceiver.
5. Confirm Tx/Rx polarity and the correct fiber strands.
6. Confirm single-mode/multimode type, wavelength, speed, and reach compatibility.
7. Check the patch panel/distribution frame and the documented path.
8. If authorized, test with a known-good compatible patch lead or optic, changing one variable at a time.
9. If the local path checks out, coordinate testing of the remote side and any intermediate fiber/splice sections.

## 4. Check compatibility

A link may fail if the two ends use incompatible speeds, transceiver types, wavelengths, fiber types, connector formats, or unsupported modules. Do not assume that an optic works in every port just because it physically fits.

## 5. Escalate with evidence

When the issue may be outside the rack or in outside-plant fiber, collect:
- Device/interface identifiers at both ends.
- Interface state, relevant logs, alarms, and error counters.
- Transceiver type and supported part details.
- Optical diagnostic readings if available.
- Patch-panel/port identifiers and the documented route.
- Changes already made and their observed results.

Then involve the field or fiber team with a clear description of the segment to test. If hardware replacement is needed, follow change control and the vendor's RMA process.

## Safety and accuracy

- Never look into a fiber connector or transceiver.
- Do not remove a live module unless the hardware and procedure explicitly permit it.
- Do not claim that a cable, optic, or panel is faulty until evidence isolates it.
- Link LEDs are useful evidence, but they do not prove that the complete end-to-end service is working.
