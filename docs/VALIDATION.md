# Lab validation checklist

Run these checks inside the lab VMs to confirm the manual setup. Results and screenshots are still to be added; the checklist itself does not record a completed test.

Keep the VirtualBox adapters on the isolated internal network before starting. Take a snapshot before changing lab settings. Do not use real passwords or connect this practice domain to a workplace network.

## Domain controller

Run in an administrator PowerShell window on HYDRA-DC:

```powershell
Get-ADDomain | Select-Object DNSRoot, DomainMode
Get-Service NTDS, DNS, CertSvc | Select-Object Name, Status
Get-ADComputer -Filter 'Name -eq "THEPUNISHER"' | Select-Object Name, Enabled
Get-SmbShare | Select-Object Name, Path
```

Check that the DNS root is `marvel.local`, the expected services are running, the client account is enabled and the intended lab share exists. A listed share alone does not prove its access permissions are correct.

## Windows 11 client

Run on THEPUNISHER while connected to the lab network:

```powershell
Get-CimInstance Win32_ComputerSystem | Select-Object Name, Domain, PartOfDomain
Get-DnsClientServerAddress -AddressFamily IPv4
Resolve-DnsName -Type SRV _ldap._tcp.dc._msdcs.marvel.local
Test-ComputerSecureChannel -Verbose
gpresult /r
```

Check that the client belongs to `marvel.local`, uses the domain controller for DNS, resolves the domain's LDAP service record and reports a valid secure channel. Review `gpresult` for the intended Group Policy. Some details require an administrator terminal.

Sign in as a normal lab user and check access to the intended SMB share. Test a second user who should not have access, and confirm the restriction works. Do not post passwords, recovery keys or unredacted account details with the results.

## Evidence still to add

- Dated, sanitised screenshots or saved command output for the setup checks.
- Separate notes for each completed attack and defensive check.
- The environment and expected result for any check that fails.

Add exercise results after running the checks. The attack and detection exercises are still incomplete.
