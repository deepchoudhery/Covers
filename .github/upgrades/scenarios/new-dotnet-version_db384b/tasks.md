# Covers .NET 10.0 Upgrade Tasks

## Overview

This document tracks the execution of the Covers solution upgrade from .NET 5.0 to .NET 10.0. All components will be upgraded simultaneously in a single atomic operation, followed by comprehensive validation.

**Progress**: 0/4 tasks complete (0%) ![0%](https://progress-bar.xyz/0)

---

## Tasks

### [ ] TASK-001: Verify prerequisites
**References**: #plan:Phase-0-Preparation

- [ ] (1) Verify .NET 10 SDK installed per #plan:Dependencies-and-Prerequisites
- [ ] (2) SDK version 10.0.x or later available (Verify)
- [ ] (3) Confirm on upgrade branch copilot/upgrade-covers-sln-dotnet-10
- [ ] (4) On correct branch with clean working tree (Verify)

### [ ] TASK-002: Atomic framework and dependency upgrade
**References**: #plan:Phase-1-Atomic-Upgrade, #plan:Package-Update-Reference, #plan:Breaking-Changes-Catalog

- [ ] (1) Update target framework to net10.0 in Covers.csproj per #plan:Step-2
- [ ] (2) Project file updated to net10.0 (Verify)
- [ ] (3) Update all 6 package references per #plan:Package-Update-Reference (Magick.NET 14.10.2, EF Core 10.0.3, SpaServices.Extensions 10.0.3)
- [ ] (4) All 6 package references updated (Verify)
- [ ] (5) Restore NuGet dependencies
- [ ] (6) All dependencies restored successfully (Verify)
- [ ] (7) Build solution and fix all compilation errors per #plan:Breaking-Changes-Catalog (WebClient→HttpClient, Magick.NET uint casts, SpaServices deprecation)
- [ ] (8) Solution builds with 0 errors (Verify)
- [ ] (9) Solution builds with 0 warnings (Verify)

### [ ] TASK-003: Verification and testing
**References**: #plan:Phase-3-Verification-Testing

- [ ] (1) Build solution in Release configuration
- [ ] (2) Release build succeeds with 0 errors and 0 warnings (Verify)
- [ ] (3) Build SPA in ClientApp directory (npm install and npm run build)
- [ ] (4) SPA builds successfully (Verify)

### [ ] TASK-004: Final commit
**References**: #plan:Source-Control-Strategy

- [ ] (1) Commit all changes with message: "Upgrade Covers solution to .NET 10.0"

---
