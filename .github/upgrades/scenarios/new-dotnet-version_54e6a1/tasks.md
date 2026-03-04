# Covers .NET 10.0 Upgrade Tasks

## Overview

This document tracks the upgrade of the Covers ASP.NET Core web application from .NET 5.0 to .NET 10.0. The upgrade uses an all-at-once approach: the single project's framework target and all NuGet packages are updated simultaneously, followed by fixing breaking changes and validating the build.

**Progress**: 0/3 tasks complete (0%) ![0%](https://progress-bar.xyz/0)

---

## Tasks

### [ ] TASK-001: Update framework target and NuGet packages
**References**: #plan:Step-1-Update-TargetFramework, #plan:Step-2-Update-Package-References, #plan:Package-Update-Reference

- [ ] (1) Update TargetFramework to net10.0 in Covers.csproj per #plan:Step-1
- [ ] (2) TargetFramework updated to net10.0 (Verify)
- [ ] (3) Update all 6 NuGet package references per #plan:Package-Update-Reference (key: Magick.NET 14.10.3, EF Core 10.0.3, SpaServices.Extensions 10.0.3)
- [ ] (4) All package references updated (Verify)
- [ ] (5) Restore NuGet dependencies
- [ ] (6) Dependencies restored successfully (Verify)
- [ ] (7) Commit changes with message: "TASK-001: Update framework target and NuGet packages"

### [ ] TASK-002: Fix breaking changes and eliminate build warnings
**References**: #plan:Step-3-Fix-Breaking-Changes, #plan:Breaking-Changes-Catalog

- [ ] (1) Build solution and fix all compilation errors per #plan:Breaking-Changes-Catalog
- [ ] (2) Solution builds with 0 errors (Verify)
- [ ] (3) Fix all build warnings (obsolete APIs, deprecations) per #plan:Breaking-Changes-Catalog
- [ ] (4) Solution builds with 0 warnings (Verify)
- [ ] (5) Commit changes with message: "TASK-002: Fix breaking changes and eliminate build warnings"

### [ ] TASK-003: Final build validation and commit
**References**: #plan:Success-Criteria, #plan:Testing-Strategy

- [ ] (1) Run final clean build to confirm 0 errors and 0 warnings
- [ ] (2) Solution builds with 0 errors and 0 warnings (Verify)
- [ ] (3) Commit changes with message: "TASK-003: Final build validation complete"

---
