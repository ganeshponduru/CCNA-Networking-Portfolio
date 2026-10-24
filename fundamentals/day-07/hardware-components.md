# Networking Hardware Components

I'm learning to identify the physical parts of routers, switches, and other networking devices and understand what each part does.

## 1. Power supply unit (PSU) and power cable

- The **power cable** connects the device to its power source.
- The **power supply unit (PSU)** converts incoming power into the power required by the device's electronics.
- Some enterprise devices support two or more PSUs for redundancy. A common design is **1+1**, where one PSU can support the load while the other is available as a backup. Exact behavior depends on the device and power configuration.
- If a PSU fails, I should check the device alarms/status indicators, power feed, logs, and vendor hardware guide before replacing anything.

## 2. Cooling and fan trays

Fans move air through the chassis to keep components within their operating temperature range. Some platforms use removable fan trays and report fan failures through LEDs, alarms, or CLI commands.

## 3. Interfaces and ports

- **RJ-45 Ethernet ports** commonly connect copper Ethernet cabling.
- **SFP/SFP+/SFP28 and other transceiver cages** accept compatible pluggable modules, depending on the platform.
- **Console ports** provide local management access, often for initial setup or recovery.
- Port appearance alone is not enough to determine supported speed or media; I need to check the model and hardware documentation.

## 4. Transceivers

A pluggable optical or copper transceiver provides the electrical/optical interface required by a link. Examples include SFP and higher-capacity form factors. Compatibility depends on factors such as supported platform, port speed, wavelength, cable/media type, and vendor qualification.

## 5. Fiber distribution panel / distribution unit

A fiber distribution panel organizes and presents fiber connections so incoming outside-plant fibers can be spliced, terminated, and patched to equipment. A panel is typically passive; it does not regenerate an optical signal simply by being a panel.

A real fiber path may look like:

`Network device → short patch lead → fiber distribution panel → splice/ODF and outside-plant fiber → remote panel → patch lead → remote device`

This is a simplified example. The actual path varies by site.

## 6. Chassis and modular components

Larger routers and switches may use replaceable modules such as:

- **Line/interface cards or I/O modules:** provide physical interfaces and associated forwarding resources.
- **Routing/control modules:** provide control-plane and system-management functions on platforms designed with separate modules.
- **Fan trays and PSUs:** provide cooling and power.
- **Blanking panels:** cover unused chassis slots where required by the manufacturer's airflow and safety design.

Module names are vendor- and platform-specific. For example, one Cisco platform may use the term RSP, while a Juniper platform may use Routing Engine; these are not universally interchangeable components.

## 7. Replaceable parts and vendor support

A **FRU (Field-Replaceable Unit)** is a component designed to be replaced in the field under the product's service procedures. If a suspected hardware fault needs vendor support, the support team may request logs, diagnostic output, part/serial details, and an RMA (Return Merchandise Authorization) process.

I should follow the organization's change process and the vendor's service guide rather than removing live hardware based only on a guess.

## What I remember

- PSU = power.
- Fan tray = cooling.
- Interface/port = physical connection point.
- Transceiver = media interface for a supported link.
- Fiber distribution panel = organizes and patches fiber paths.
- Line card = interfaces in a modular chassis.
- Console port = local management access.
