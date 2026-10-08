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

The domain controller and the client sit on an isolated VirtualBox internal network,
and the Kali machine goes on the same network for testing, so the lab is fully
contained. The Marvel hostnames just follow the course naming convention.

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

## What the lab is for

The lab is set up so I can work through these techniques as part of my PNPT study. For
each one the aim is to look at both sides: how the attack works, and how a defender
would detect and prevent it.

- LLMNR and NBT-NS poisoning, and why turning LLMNR off removes the problem.
- SMB relay, and how enforcing SMB signing stops it.
- Kerberoasting the service account, and why long service-account passwords or group
  managed service accounts matter.
- Reading the Windows security event logs to see what each attack looks like from the
  blue-team side.

## Tools the lab uses

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

- [x] Domain controller built and promoted, with DNS and AD CS
- [x] Windows 11 client joined to the domain
- [x] Users, a service account, a file share and a Group Policy in place
- [ ] Connect the Kali machine and work through each attack
- [ ] Write up each attack and its detection as its own set of notes
- [ ] Add a second client to practise lateral movement
- [ ] Add screenshots and a cleaner network diagram

---

Built and maintained by Hasibur Rahaman Hamim as a learning project. For education only,
in an isolated lab I control.
