# UDP vs TCP

## Differences

| TCP                                        | UDP                                        |
|--------------------------------------------|--------------------------------------------|
| Connection-oriented (Data arrives inorder) | Connection less (data can be out of order) |
| Three-way handshake                        | No handshake                               |
| Header (20 bytes)                          | Header (8 bytes)                           |
| Used for  Point-to-point communicated      | Used for Unicast & Multicast & Broadcast   |
| Congestion control                         | no congestion control                      |
| Reliable                                   | Packets can be lost                        |
| Flow control                               | no flow control                            |

## Use cases of TCP & UDP


| TCP                                      | UDP                       |
|------------------------------------------|---------------------------|
| Serving pages in web servers using HTTPS | DNS Resolution            |
| Downloading a file using FTP             | No handshake              |
| Sending a email using SMTP               | Internet routing with RIP |
| 

## References
* <https://www.linkedin.com/posts/alexxubyte_tcp-vs-udp-activity-7049406670107484160-2RK0?utm_source=share&utm_medium=member_desktop>