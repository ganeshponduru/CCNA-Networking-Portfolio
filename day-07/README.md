# Day 7 — Networking Hardware and Physical Connectivity

I'm learning what sits behind a network link: power supplies, interfaces, transceivers, fiber cabling, distribution panels, chassis modules, and the ways engineers investigate a physical-link problem.

## Notes

- [Networking hardware components](hardware-components.md)
- [Fiber, SFP transceivers, and connectors](fiber-optics-and-transceivers.md)
- [Chassis, line cards, and redundancy](chassis-and-modular-hardware.md)
- [Physical-link troubleshooting checklist](physical-link-troubleshooting.md)
- [Console access basics](console-access-basics.md)
- [Revision questions](practice-questions.md)

## What I learned

- A network device needs power, cooling, interfaces, and the right transceivers/cables for the intended link.
- SFP-family modules are transceivers; their supported speed, wavelength, fiber type, and reach depend on the exact module and platform.
- Fiber paths may include patch leads, distribution panels, splices, and outside-plant cable. The device-to-device drawing is a logical view, not necessarily a direct cable run.
- Modular chassis can use line/interface cards and separate control-plane or routing-engine modules. Terminology differs by vendor and platform.
- Redundant power supplies or control modules can improve availability, but the actual redundancy behavior depends on the model and configuration.
- For an interface that stays down, I should verify both ends, optics, fiber polarity, compatibility, port configuration, alarms, and the physical path before assuming a cause.

## Practice status

These are study notes based on the lesson transcript and the supplied hardware handout. I have **not** marked a physical hardware troubleshooting lab or a console-login lab as completed here. I will add evidence when I personally perform and document those steps.

## Technical clarifications I kept in mind

- SFP reach is not universally 35–40 km. It depends on the transceiver specification, fiber, wavelength, connector losses, and link budget.
- An SFP slot does not necessarily accept every SFP or every speed; compatibility is platform- and port-dependent.
- Fiber connectors include SC, LC, ST, FC, and MPO/MTP families. The letters do not mean “single core” or “single-core patch card.”
- Fiber cable splices are made when needed for installation, repair, or cable-length constraints; they are not necessarily placed every 2 km.
- A fiber distribution panel is generally passive, but the whole optical path may include active equipment such as amplifiers or DWDM systems elsewhere.
- A router/switch operating system is not universally “installed on an RSP.” Hardware architecture and module names vary across vendors and product families.
