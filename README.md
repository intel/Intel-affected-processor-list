# Intel Affected Processors and TCB Recovery Lists

This repository contains machine-readable versions of two tables published on the Intel Platform Security Guidance website:

| File | Source table |
| --- | --- |
| `Intel_affected_processor_list.csv` | [Affected Processors: Guidance for Security Issues on Intel® Processors](https://www.intel.com/content/www/us/en/developer/topic-technology/software-security-guidance/processors-affected-consolidated-product-cpu-model.html) |
| `Intel_TCB_recovery_list.csv` | [Trusted Computing Base Recovery Attestation](https://www.intel.com/content/www/us/en/developer/topic-technology/software-security-guidance/trusted-computing-base-recovery-attestation.html) |

These machine-readable versions are provided under the terms of the [0BSD license](https://opensource.org/license/0bsd). Please refer to the [Platform Security Guidance](https://www.intel.com/content/www/us/en/developer/topic-technology/software-security-guidance/overview.html) website for the most up-to-date information. The machine-readable versions downloadable from this repository are the official machine-readable versions and are subject to the same footnotes as the website.

## Affected Processors

This table lists the security issues that affect currently supported Intel® products, and the mitigation required for each.

### End of Servicing Lifetime
Processors that have met the End-of-Servicing-Lifetime (EOSL) milestone may not be listed in the table and mitigation status of EOSL processors may not be evaluated. More information on processor servicing changes may be found in this help article https://www.intel.com/content/www/us/en/support/topics/support-and-servicing-for-processors.html. Some processors may have extended servicing, which is servicing provided after broad market servicing concludes.  Extended servicing is available to customers who have extended servicing agreements with Intel. Intel does not have extended servicing for all processors. For customers interested in extended servicing, please contact your Intel representative.

### Special note for IPU 26.3
For Meteor Lake (`06_AAH`), MCU version `0x2a` listed in IPU 26.3 is currently delivered through BIOS and is not yet distributed through OS Patch Loading (OSPL). An OSPL-supported version will be available in September 2026.

### Legend
**SW**: Software mitigation  
**BIOS**: BIOS changes needed  
**MCU**: Microcode mitigation  
**MCU_BIOS**: Microcode mittigation should be loaded by BIOS  
**HW + SW**: Mitigated in hardware, but software needs to take actions

## Trusted Computing Base Recovery

This table identifies processors affected by specific Trusted Computing Base Recovery (TCB-R) events, and gives the corresponding Security Advisory (SA), TCB-R identifier, and the recovery action required for Intel® SGX and Intel® TDX attestation to succeed.

Each vulnerability column header ends with the TCB-R counter in which that issue was addressed, for example `... - INTEL-SA-01449 - TCB-R 22`. The counters map to Intel Platform Update (IPU) releases as follows.

| TCB-R counter | IPU | Date |
| --- | --- | --- |
| 22 | IPU 26.3 | August 2026 |
| 21 | IPU 26.1 | February 2026 |
| 20 | IPU 25.3 | August 2025 |
| 19 | IPU 25.2 | May 2025 |
| 18 | IPU 24.3 / UPLR2 | September, November 2024 |
| 17 | IPU 24.1 | February 2024 |

`TCB-R TBD` indicates that a TCB-R counter has not yet been assigned for that issue. The header will be updated once the counter is determined.

### Legend
**MCU**: Mitigation requires a microcode update. It is runtime effective.  
**MCU_BIOS_TCBR**: Mitigation requires an updated BIOS carrying new microcode only loadable in flash to be reflected in Intel SGX and Intel TDX attestation and TCB-R completion.  
**MCU_OSPL_SGX_TCBR**: Mitigation eligible for Intel SGX runtime update. After OS Patch Loading (OSPL), tear down all enclaves, perform EUPDATESVN, and then relaunch any enclave.  
**MCU_OSPL_TDX_TCBR**: Mitigation eligible for Intel TDX runtime update. A new microcode update will be attestable.  
**ACM_BIOS**: Mitigation requires an Authenticated Code Module (ACM) update, which is part of a BIOS update. A BIOS update and cold reset are required.  
**ACM_SINIT**: Mitigation requires an update to Intel® Trusted Execution Technology, which can be updated at runtime for effectiveness. All virtual machines will be lost, and a cold reset is required for attestation and complete TCB-R.  
**TDX_M**: Mitigation requires an Intel TDX module update. When TDPRESERVING is supported by the VMM, the update is runtime eligible and attestable; otherwise a warm reset is required.  
**SGX_E**: Mitigation requires updating and reloading an Intel SGX architectural enclave. A warm reset is not necessary for attestability and TCB-R completion.  
**SGX_SW**: Mitigation requires updating and reloading the Intel SGX software collection, for example Intel® Data Center Attestation Primitives (Intel® DCAP), required for attestation.  
**Not Affected**: Products are not affected or mitigated through hardware and may not be enumerated.  
**No Planned Mitigation**: An issue exists but no mitigation is planned.

Where a specific version is required to complete recovery, it is given in parentheses after the action. For example `TDX_M (1.5.34)` names an Intel TDX module version, `SGX_SW (DCAP v1.25)` an Intel DCAP version, and `ACM_BIOS (ACTM_EGS_03.00.431)` an Alias Tracking Trusted Module (ACTM) version. A minimum Security Version Number, where one applies, is given in the `MCU Update` column, for example `0x01000434 (SVN7 or above)`.

## Cross-referencing the two lists

The two tables describe different aspects of the same processors. The Affected Processors List identifies applicable vulnerabilities and mitigations, while the TCB-R table identifies TCB recovery events and the processors they affect. Since both tables are keyed on CPUID, they can be correlated to determine which mitigations apply to a processor in addition to those associated with TCB-R events

Correlating the two tables lets you determine:

- Which processors are associated with a particular TCB-R event.
- Which Security Advisories require TCB Recovery.
- Whether additional Security Advisories affect the same processor.
- Which mitigations remain applicable for those processors.

For example, Emerald Rapids (5th Generation Intel® Xeon® Scalable processors, `CPUID C06F2`) appears in the TCB-R list with several recovery-related advisories:

- INTEL-SA-01436 and INTEL-SA-01419 require `TDX_M (1.5.34)`.
- INTEL-SA-01442 requires `MCU_BIOS_TCBR`.
- INTEL-SA-01439 requires `ACM_BIOS (ACTM_EGS_03.00.431)`.

Looking up the same `CPUID C06F2` in the Affected Processors list shows further advisories that apply to the same part and the mitigation each needs, such as INTEL-SA-01247 and INTEL-SA-01166, which both require `MCU`.

An advisory can also appear in both tables with different meanings. INTEL-SA-01103 (RAPL, CVE-2024-23984) is listed as `MCU` in the Affected Processors list, meaning a microcode update mitigates it, and as `MCU_OSPL_TDX_TCBR MCU_OSPL_SGX_TCBR` in the TCB-R list, describing what is additionally required for that microcode update to become attestable.
