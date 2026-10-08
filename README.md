# Active Directory Security Home Lab

A small Windows Active Directory lab I built in VirtualBox to practise how common
AD attacks work, and just as importantly, how to spot and stop them. Everything runs
on an isolated internal network with no route to the internet or my real network.

I put this together while studying for the PNPT (Practical Network Penetration
Tester), following TCM Security's Practical Ethical Hacking material. It is a learning
lab and still a work in progress.

## What is in the lab

| Machine | OS | Role |
|---------|-----|------|
| HYDRA-DC | Windows Server 2022 | Domain controller for the `marvel.local` forest (AD DS, DNS, AD CS) |
| THEPUNISHER | Windows 11 | Domain-joined workstation |
| Kali | Kali Linux | Attacker machine |

All three sit on a VirtualBox internal network, so the lab is fully contained. The
Marvel hostnames just follow the course naming convention.

```
            Internal network (isolated, no internet)
   +-----------------+   +------------------+   +-------------+
   |   HYDRA-DC      |   |   THEPUNISHER    |   |    Kali     |
   | Windows Server  |   |   Windows 11     |   |   attacker  |
   | AD DS, DNS,     |---|  domain-joined   |---|             |
   | AD CS           |   |     client       |   |             |
   +-----------------+   +------------------+   +-------------+
```

## How it is set up

- Built a new AD forest (`marvel.local`) on the domain controller, with DNS.
- Added Active Directory Certificate Services.
- Joined a Windows 11 client to the domain.
- Created domain users, a service account, and a basic SMB file share.
- Used Group Policy to manage settings across the domain.

To make the practice realistic I deliberately left a few common misconfigurations in
place, the same mistakes that show up in real networks. No real passwords are in this
repo. The lab credentials are throwaway values that only exist on the isolated network.

## What I practised

For each technique I looked at both sides: running it in the lab, then working out how
a defender would detect and prevent it.

- LLMNR and NBT-NS poisoning, capturing hashes with Responder, and why turning LLMNR
  off removes the problem.
- SMB relay, and how enforcing SMB signing stops it.
- Kerberoasting the service account, cracking the ticket offline, and why long
  service-account passwords or group managed service accounts matter.
- Reading the Windows security event logs to see what each attack looks like from the
  blue-team side.

## Tools

Kali, Responder, Impacket, Nmap, Wireshark, Hashcat, and the built-in Windows and AD
admin tools (PowerShell, Active Directory Users and Computers, Group Policy Management).

## Defensive takeaways

- Disable LLMNR and NBT-NS. They leak authentication for almost no benefit.
- Enforce SMB signing so relayed authentication fails.
- Give service accounts long random passwords, or use group managed service accounts.
- Never put passwords in account description fields.
- Keep local admin passwords unique per machine so one password does not open the
  whole domain.

## Status and next steps

- [x] Domain controller, client and attacker machines built and talking to each other
- [x] Core AD services, users, share and a service account in place
- [ ] Write up each attack and its detection as its own set of notes
- [ ] Add a second client to practise lateral movement
- [ ] Add screenshots and a cleaner network diagram

---

Built and maintained by Hasibur Rahaman Hamim as a learning project. For education only,
in an isolated lab I control.
