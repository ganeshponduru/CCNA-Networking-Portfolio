# References and Attribution

## Learning source

E. Ramesh Goud, TungaBadra Networks — Day 1 lesson, “How Data Travels, Network Reference Models & Packet Header Analysis.” These notes are an original study summary of the lesson topics, not a reproduction of the instructor's handout or transcript.

## Technical references

- IANA, Service Name and Transport Protocol Port Number Registry: https://www.iana.org/assignments/service-names-port-numbers/service-names-port-numbers.xhtml
- RFC 9293, Transmission Control Protocol (TCP): https://www.rfc-editor.org/rfc/rfc9293
- RFC 768, User Datagram Protocol: https://www.rfc-editor.org/rfc/rfc768
- RFC 791, Internet Protocol: https://www.rfc-editor.org/rfc/rfc791
- RFC 826, An Ethernet Address Resolution Protocol: https://www.rfc-editor.org/rfc/rfc826
- RFC 9114, HTTP/3: https://www.rfc-editor.org/rfc/rfc9114
- Wireshark User's Guide: https://www.wireshark.org/docs/wsug_html_chunked/

## Historical note

An often-repeated ARPANET anecdote describes a 1969 UCLA-to-SRI test in which “LOGIN” was attempted and only “LO” reportedly arrived before a crash. Treat it as a historical anecdote about an early system failure, not proof that unstructured data inherently crashes networks. Verify the details against a reliable historical source before using it in a formal report.

## Accuracy notes

- OSI is a conceptual reference model; real stacks do not always map neatly to one layer per protocol.
- TCP reliability is not encryption; TLS or other security protocols provide confidentiality/authentication.
- TCP/UDP IP protocol numbers (6 and 17) differ from TCP/UDP port numbers.
- DNS can use UDP and TCP; HTTP/3 uses QUIC over UDP.
- Packet captures show traffic visible at the capture point and may be affected by filters, offloads, encryption, and truncation.