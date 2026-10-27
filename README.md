# Enterprise SMB Ransomware-in-Place: Incident Forensic Investigation

An individual SOC incident investigation report and technical evidence pack analyzing an enterprise ransomware campaign (`Project Enterprise 18.pcap`) using Wireshark.
---

## Executive Summary
An in-depth forensic examination of network traffic revealed a coordinated, automated ransomware attack targeting the private internal subnet (`10.18.0.0/24`). Workstation `10.18.0.107` established an authenticated SMB 3.0.2 session with the primary file server (`FS01` at `10.18.0.20`), executing over 17,000 rapid file-system operations. Rather than exfiltrating data externally, the threat actor performed **client-side encryption-in-place**, utilizing temporary staging buffers (`~RF*.tmp`) and overwriting files with high-entropy ciphertext before appending the `.arc2018` extension.

## Technical Investigation & Methodology
- **Protocol Analysis:** Filtered and tracked SMB2 commands (`smb2.cmd == 5` for Create, `smb2.cmd == 8` for Read, `smb2.cmd == 9` for Write, and `smb2.cmd == 17` for SetInfo/Rename).
- **Noise Reduction:** Successfully separated non-causal RFC 5737 test range telemetry and external DNS background queries from true indicators of compromise.
- **Hypothesis Testing:** Differentiated legitimate backup/archive traffic (`10.18.0.30`) from aggressive ransomware execution loops (`10.18.0.107`).

## Repository Structure
- `Mercy Olalere Network Traffic Analysis.pdf`: Contains the complete polished PDF incident report.
- `smb_ransomware_detection.yml`: Contains the custom behavioral Sigma detection rule.

## Key Indicators of Compromise (IoCs)
- **Attacker Workstation:** `10.18.0.107`
- **Target File Server:** `10.18.0.20` (`FS01`)
- **Staging Pattern:** `~RF*.tmp`
- **Malicious Extension:** `.arc2018`

---
*Prepared by Mercy Itunuoluwa Olalere*
