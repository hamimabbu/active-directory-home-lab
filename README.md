# Active Directory home lab

I built this VirtualBox lab while studying for the PNPT, following TCM Security's Practical Ethical Hacking course. It uses an isolated internal network with no route to the internet or my normal network.

The domain controller and Windows client are set up. The attack exercises and detection notes are still to do.

## Machines

| Machine | OS | Role |
| --- | --- | --- |
| HYDRA-DC | Windows Server 2022 | Domain controller for `marvel.local`, with AD DS, DNS and AD CS |
| THEPUNISHER | Windows 11 | Domain-joined client |
| Kali | Kali Linux | Planned attack VM for the exercises |

The Windows machines share the internal VirtualBox network. Kali will join that network for the exercises. The hostnames follow the course naming convention.

```text
                 Isolated internal network
   +-----------------+   +------------------+   +-------------+
   |   HYDRA-DC      |   |   THEPUNISHER    |   |    Kali     |
   | Windows Server  |   |   Windows 11     |   |  planned    |
   | AD DS, DNS,     |---|  domain-joined   |---| attack VM   |
   | AD CS           |   |     client       |   |             |
   +-----------------+   +------------------+   +-------------+
```

## Setup completed

- Created the `marvel.local` forest and configured DNS.
- Added Active Directory Certificate Services.
- Joined the Windows 11 client to the domain.
- Created users, a service account and an SMB file share.
- Applied a Group Policy.

The lab includes deliberate misconfigurations for practice. Credentials are throwaway values used only in this isolated environment; real passwords are not included in the repository.

## Planned exercises

For each exercise, I plan to record the attack, the Windows events it produces and the result after changing the relevant settings:

- LLMNR and NBT-NS poisoning, then checks with those protocols disabled.
- SMB relay, with a comparison of SMB signing settings.
- Kerberoasting the service account, followed by a review of service-account passwords and group managed service accounts.

Other checks include keeping passwords out of account descriptions and using different local administrator passwords on each machine.

The tools for these exercises are Kali, Responder, Impacket, Nmap, Wireshark and Hashcat, alongside PowerShell, Active Directory Users and Computers, and Group Policy Management.

## Checking the lab

The [validation checklist](docs/VALIDATION.md) covers domain membership, DNS, the secure channel, Group Policy and file-share access. It needs to be run inside the VMs. Screenshots and saved results are still to be added.

This repository contains the write-up rather than a VM image or automated installer.

## Still to do

- [ ] Connect Kali and work through each attack exercise.
- [ ] Write separate attack and detection notes.
- [ ] Add a second Windows client for lateral movement practice.
- [ ] Add sanitised screenshots and a cleaner network diagram.

Built by Hasibur Rahaman Hamim for practice on an isolated lab I control.
