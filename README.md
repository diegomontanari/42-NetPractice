*This project has been created as part of the 42 curriculum by dmontana.*

# NetPractice

## Description

NetPractice is a 42 project that introduces the basics of computer networking through hands-on exercises. The networks are **simulated**: a training interface, opened in the web browser, shows 10 broken network diagrams (hosts, switches, routers, the Internet). For each level the goal is to fix the configuration so that every objective is reached.

What the project teaches:

- how TCP/IP addressing works (IPv4 addresses, subnet masks, CIDR notation);
- how to split a network into subnets and compute network address, broadcast address and usable host range;
- the role of the default gateway and of routing tables (destination + next hop);
- how routers and switches connect devices, and what differs between public and private addresses.

There is no code to compile: the deliverable is the exported configuration of each of the 10 levels.

## Instructions

### Running the training interface

1. Download the archive attached to the project page and extract it in any folder.
2. In that folder run:

   ```sh
   ./run.sh
   ```

   The script starts a local web server and opens the interface in the default browser.
3. If `run.sh` does not work, start the server manually and open the page yourself:

   ```sh
   python3 -m http.server 49242
   # then open http://localhost:49242
   ```

   (the port can be changed).
4. **Enter your 42 login** in the field on the welcome page: this generates your personal configuration. The "evaluation" tab generates random configurations, also used during defenses.

### Solving and exporting the levels

- Edit the unshaded fields until the objectives at the top of the page are met.
- Use **Check again** to validate; read the logs at the bottom of the page if it fails (missing gateway, invalid IP, ...).
- When a level is solved, click **Get my config** to download its configuration **before** moving to the next level.

### Submission

- Place the **10 exported configuration files (one per level) at the root of this repository**, together with this `README.md`.
- Double-check file names before pushing; only the repository content is evaluated.
- During the defense you will solve three random levels in a limited time, without external tools (a simple calculator such as `bc` is tolerated).

## Resources

### Networking concepts studied

- TCP/IP addressing and IPv4 representation
- Subnet masks, CIDR notation, network and broadcast addresses
- Default gateways
- Routers, routing tables and next hops
- Switches and local networks (LAN / WAN)
- IP vs MAC addresses
- OSI layers
- Public vs private address ranges

### References

- [42 Cursus guide - NetPractice](https://42-cursus.gitbook.io/guide/4-rank-04/netpractice)
- [NetPractice: theory and cheat sheet (Digital Brain, in Italian)](https://build-sigma-coral.vercel.app/docs/42/NetPractice/intro)
- [Sai davvero come funziona Internet? NetPractice (42)](https://diegomontanari.com/2026/09/06/sai-davvero-come-funziona-internet-net-practice-42/) - article about the project by the author (in Italian)
- Wikipedia: [IPv4](https://en.wikipedia.org/wiki/IPv4), [Subnetwork](https://en.wikipedia.org/wiki/Subnetwork), [Default gateway](https://en.wikipedia.org/wiki/Default_gateway), [OSI model](https://en.wikipedia.org/wiki/OSI_model)

### Use of AI

AI was used as a study aid, not to solve the levels for me: to explain concepts (subnet masks, gateways, routing), to organize my notes into the Digital Brain guide, and to check my reasoning on tricky cases. Every level was solved and verified by me in the training interface, and the AI-generated explanations were reviewed and discussed with peers.
