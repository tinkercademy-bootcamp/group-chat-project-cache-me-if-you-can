# Group Exercise

## Overview
Come up with an interesting group project centered around a chat app over the TCP protocol.

## Planning
- Come up with a detailed timeline for the features you want to support
- Each team will work on one task. 

## Tasks
1. Create the directory structure and organization of the files
2. Create a test framework which can create many client connections to the chat server and measure the transfer speed
3. Create a method to benchmark the performance, say for example by using `perf` and [FlameGraph](https://github.com/brendangregg/FlameGraph)
4. Measure the performance for that chat server sending chat messages to a large number of clients using TCP, vs sending the chat messages by using UDP multicast

## Project Management Questions
- How will you handle branching and Pull Requests?
- How do you determine which PRs get accepted by the group, and which PRs get accepted into the main branch?

## Technical Considerations
### Data Structures and Formats
- What data structures would you need to send and receive from the server? 
- What data format should the data structures be sent in?
- Keeping in mind that in HFT, we are seeking maximum ultra low latency programming, what data format should we use to send our data?
- What if you had to scale this chat program up to support millions of simultaneous users?

### Performance and Measurement
- I recommend experimenting with different data formats and measuring the performance, instead of guessing.
- I will be curious to learn what you find out about the limitations of various forms of measurement and what can be done about them.
- The Bonus section from `exercise-3` of `tt-chat` will be helpful in this process

### Technical Concepts to Study
- It will be relevant to learn about why structs and classes need to have padding and byte alignment
- It will be relevant to learn about `#pragma pack` and `__attribute__((packed))` 
- It will be relevant to learn more about `inet_ntop()` and `inet_pton()`.
- Similarly, `htons()` and `ntohs()`, `htonl()` and `ntohl()`
- What is network byte order and what is system byte order, in the context of these functions?

### HFT-Specific Considerations
- It will be relevant to watch this video: [Type Punning in C++](https://www.youtube.com/watch?v=_qzMpk-22cc)
- With an additional explanation that for HFT, for performance reasons the preference is to `std::memcpy` only a small metadata header
  - Which includes a checksum. 
  - If the checksum passes and the size of the received data matches the metadata 
  - It is ok to use reinterpret_cast on the remaining payload.
  - This is ok in a HFT context because the same company controls both sides of the client and server and can control the platform and compiler flags used for both builds

### Learning Resources
- There are some unofficial guides that are more beginner friendly than the official RFCs or the man pages.
- For example, a well regarded introduction to sockets is [Beej's guide to Network Programming](https://beej.us/guide/bgnet/html/)
- How do you go about finding such good unofficial guides? 
- Note that Beej's guide is not completely up-to-date.
- epoll is considered a better replacement for select and poll
- How would you go about testing the performance of the chat server sending its messages by TCP, vs multicasting by UDP?

## Reference Materials
- [RFC 1112: IP Multicasting](https://www.rfc-editor.org/rfc/rfc1112)
- [RFC 768: UDP](https://datatracker.ietf.org/doc/html/rfc0768)
- [RFC 8085: UDP Usage Guidelines](https://datatracker.ietf.org/doc/html/rfc8085)
- [tldp Multicasting](https://tldp.org/HOWTO/Multicast-HOWTO.html) (note that the above is titled Multicasting over TCP/IP but actually discusses multicasting over UDP, this is explained in section 2 of the guide)
