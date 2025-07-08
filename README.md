# Linux Kernel - Mapping Desynchronization Vulnerability in `remap_pfn_range()`

**Author:** Matteu Olivieri Bastiani  
**Date:** 2025-07-08  
**Status:** Public Disclosure  
**CVE:** Pending Assignment  
**Severity:** High



## Summary

A **mapping desynchronization** vulnerability exists in the Linux kernel memory management subsystem (`mm/mmap.c`) in the function `remap_pfn_range()`. Due to insufficient synchronization between `vm_area_struct` flags (`vma->vm_flags`) and the effective page protections (`pgprot`), unprivileged processes or kernel drivers can create inconsistent memory mappings, potentially leading to **Write-After-Free**, **arbitrary writes**, or **W^X policy bypass**.



## Affected Component

- **Subsystem:** Memory Management
- **File:** `mm/mmap.c`
- **Function:** `remap_pfn_range()`
- **Kernel Versions:** Confirmed in Linux 5.x and 6.x (mainline not patched as of this disclosure)



Potential Impact

    Write-After-Free: Under certain driver scenarios that remap PFN ranges while relying on VMA flags.

    W^X Policy Bypass: Mapping read-only pages while marking them writable in vma.

    Arbitrary Write Primitive: In configurations with custom drivers that fail to re-validate protections.

    Denial of Service: Kernel crashes if unexpected write attempts occur.


Proof of Concept

No working exploit is included.
The vulnerability is demonstrated through static analysis of remap_pfn_range() logic and inconsistencies between flags.

Timeline

    2025-07-08: Vulnerability discovered and analyzed.

    2025-07-08: Public disclosure via GitHub.

    2025-07-08: CVE requested from DWF.
