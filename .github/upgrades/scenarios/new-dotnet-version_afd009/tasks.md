# Covers .NET 10.0 Upgrade Tasks

## Overview

This document tracks the execution of the Covers upgrade from .NET 5.0 to .NET 10.0. All framework updates, package updates, and code changes will be performed in a single atomic operation, followed by comprehensive testing and validation.

**Progress**: 3/3 tasks complete (100%) ![100%](https://progress-bar.xyz/100)

---

## Tasks

### [x] TASK-001: Atomic framework and dependency upgrade
**References**: #plan:Phase-1, #plan:Phase-2, #plan:Package-Update-Reference, #plan:Breaking-Changes

- [x] (1) Update target framework to net10.0 in Covers.csproj per #plan:1.1
- [x] (2) Project file updated to target framework (Verify)
- [x] (3) Update package references per #plan:Package-Update-Reference (Magick.NET 14.10.2, EF Core 10.0.3, ASP.NET Core 10.0.3)
- [x] (4) All package references updated (Verify)
- [x] (5) Restore all NuGet dependencies
- [x] (6) Dependencies restored successfully (Verify)
- [x] (7) Commit changes with message: "TASK-001: Update framework and dependencies to .NET 10.0"

**Status**: ✅ SUCCESS
**Summary**: Updated target framework to net10.0, upgraded Magick.NET to 14.10.2 (security fix), upgraded all EF Core and ASP.NET Core packages to 10.0.3, restored dependencies successfully.

### [x] TASK-002: Build validation and error resolution
**References**: #plan:Phase-3, #plan:Breaking-Changes

- [x] (1) Build solution and fix all compilation errors per #plan:Breaking-Changes (Magick.NET uint casts, obsolete APIs)
- [x] (2) Solution builds with 0 errors (Verify)
- [x] (3) Solution builds with 0 warnings (Verify)
- [x] (4) Commit changes with message: "TASK-002: Fix breaking changes and resolve build errors"

**Status**: ✅ SUCCESS
**Summary**: Fixed Magick.NET uint casting issues in CoverController.cs, replaced obsolete WebClient with HttpClient in SpotifyService.cs and AlbumScanner.cs, added HttpClient dependency injection. Solution builds with 0 errors and 0 warnings.

### [x] TASK-003: Final verification and commit
**References**: #plan:Phase-4, #plan:Testing-Strategy

- [x] (1) Verify all security vulnerabilities resolved (especially Magick.NET)
- [x] (2) No security vulnerabilities in packages (Verify)
- [x] (3) Commit changes with message: "TASK-003: Complete .NET 10.0 upgrade"

**Status**: ✅ SUCCESS
**Summary**: Verified no security vulnerabilities remain in packages. Magick.NET security issue resolved. Final build verification passed with 0 errors and 0 warnings.

---

## Upgrade Summary

**All tasks completed successfully! ✅**

### Changes Made:
1. **Target Framework**: net5.0 → net10.0
2. **Package Updates**:
   - Magick.NET-Q8-AnyCPU: 7.22.3 → 14.10.2 (🔴 Security fix)
   - Microsoft.AspNetCore.SpaServices.Extensions: 5.0.1 → 10.0.3
   - Microsoft.EntityFrameworkCore.Design: 5.0.1 → 10.0.3
   - Microsoft.EntityFrameworkCore.Proxies: 5.0.1 → 10.0.3
   - Microsoft.EntityFrameworkCore.Sqlite: 5.0.1 → 10.0.3
   - Microsoft.EntityFrameworkCore.Tools: 5.0.1 → 10.0.3
3. **Code Changes**:
   - Fixed Magick.NET uint casting (Controllers/CoverController.cs)
   - Replaced WebClient with HttpClient (Services/SpotifyService.cs)
   - Replaced WebClient with HttpClient (BackgroundServices/AlbumScanner.cs)
   - Added HttpClient dependency injection to services

### Final Status:
- ✅ Build: 0 errors, 0 warnings
- ✅ Security: No vulnerabilities
- ✅ All tasks complete

