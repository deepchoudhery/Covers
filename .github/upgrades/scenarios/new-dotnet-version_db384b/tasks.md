# Covers .NET 10.0 Upgrade Tasks

## Overview

This document tracks the execution of the Covers solution upgrade from .NET 5.0 to .NET 10.0. All components will be upgraded simultaneously in a single atomic operation, followed by comprehensive validation.

**Progress**: 4/4 tasks complete (100%) ![100%](https://progress-bar.xyz/100)

---

## Tasks

### [✓] TASK-001: Verify prerequisites
**References**: #plan:Phase-0-Preparation
**Status**: ✅ **Complete**

- [✓] (1) Verify .NET 10 SDK installed per #plan:Dependencies-and-Prerequisites
- [✓] (2) SDK version 10.0.x or later available (Verify) - **SDK 10.0.103 confirmed**
- [✓] (3) Confirm on upgrade branch copilot/upgrade-covers-sln-dotnet-10
- [✓] (4) On correct branch with clean working tree (Verify) - **Confirmed**

### [✓] TASK-002: Atomic framework and dependency upgrade
**References**: #plan:Phase-1-Atomic-Upgrade, #plan:Package-Update-Reference, #plan:Breaking-Changes-Catalog
**Status**: ✅ **Complete**

- [✓] (1) Update target framework to net10.0 in Covers.csproj per #plan:Step-2
- [✓] (2) Project file updated to net10.0 (Verify) - **Updated**
- [✓] (3) Update all 6 package references per #plan:Package-Update-Reference (Magick.NET 14.10.2, EF Core 10.0.3, SpaServices.Extensions 10.0.3)
- [✓] (4) All 6 package references updated (Verify) - **All updated**
- [✓] (5) Restore NuGet dependencies
- [✓] (6) All dependencies restored successfully (Verify) - **Restored in 15.2 sec**
- [✓] (7) Build solution and fix all compilation errors per #plan:Breaking-Changes-Catalog (WebClient→HttpClient, Magick.NET uint casts, SpaServices deprecation)
- [✓] (8) Solution builds with 0 errors (Verify) - **✅ 0 errors**
- [✓] (9) Solution builds with 0 warnings (Verify) - **✅ 0 warnings**

**Breaking Changes Fixed**:
- ✅ Magick.NET Width/Height: Added explicit `(uint)` casts in CoverController.cs
- ✅ WebClient obsolescence: Registered `AddHttpClient()` in Startup.cs
- ✅ WebClient → HttpClient: Updated SpotifyService.cs and AlbumScanner.cs to use `IHttpClientFactory`

### [✓] TASK-003: Verification and testing
**References**: #plan:Phase-3-Verification-Testing
**Status**: ✅ **Complete**

- [✓] (1) Build solution in Release configuration
- [✓] (2) Release build succeeds with 0 errors and 0 warnings (Verify) - **✅ Success (1.96s)**
- [✓] (3) Build SPA in ClientApp directory (npm install and npm run build)
- [✓] (4) SPA builds successfully (Verify) - **✅ Built (with warnings for accessibility, but successful)**

**SPA Build Fix**:
- ✅ Added `NODE_OPTIONS=--openssl-legacy-provider` to support Node.js 20+ with legacy webpack

### [✓] TASK-004: Final commit
**References**: #plan:Source-Control-Strategy
**Status**: ✅ **Complete**

- [✓] (1) Commit all changes with message: "Upgrade Covers solution to .NET 10.0" - **Commit e78d3fc**

---

## Upgrade Summary

### ✅ Upgrade Complete - All Tasks Successful

**Framework**: net5.0 → net10.0 ✅  
**Build Status**: 0 errors, 0 warnings ✅  
**Package Updates**: 6/6 successful ✅  
**Security Fix**: Magick.NET vulnerability resolved ✅  
**Breaking Changes**: All resolved ✅

### Changes Made

**Project Files**:
- `Covers.csproj`: Updated TargetFramework to net10.0
- `Covers.csproj`: Updated 6 package references

**Code Changes**:
- `Startup.cs`: Added `services.AddHttpClient()` for DI
- `SpotifyService.cs`: Replaced WebClient with HttpClient via IHttpClientFactory
- `AlbumScanner.cs`: Replaced WebClient with HttpClient via IHttpClientFactory
- `CoverController.cs`: Added uint casts for Magick.NET Width/Height properties
- `ClientApp/package.json`: Added OpenSSL legacy provider for Node.js 20+ compatibility

**Packages Updated**:
1. Magick.NET-Q8-AnyCPU: 7.22.3 → 14.10.2 (SECURITY FIX)
2. Microsoft.EntityFrameworkCore.Design: 5.0.1 → 10.0.3
3. Microsoft.EntityFrameworkCore.Proxies: 5.0.1 → 10.0.3
4. Microsoft.EntityFrameworkCore.Sqlite: 5.0.1 → 10.0.3
5. Microsoft.EntityFrameworkCore.Tools: 5.0.1 → 10.0.3
6. Microsoft.AspNetCore.SpaServices.Extensions: 5.0.1 → 10.0.3

**Build Results**:
- Debug build: ✅ 0 errors, 0 warnings (2:00.39 including npm install)
- Release build: ✅ 0 errors, 0 warnings (1.96s)
- SPA build: ✅ Successful (with linting warnings)

---
