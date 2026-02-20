# Upgrade Plan: Covers.sln to .NET 10.0

**Date**: 2026-02-20  
**Source Framework**: net5.0  
**Target Framework**: net10.0

---

## Executive Summary

This plan outlines the upgrade of the Covers solution from .NET 5.0 to .NET 10.0. The solution contains a single ASP.NET Core project (`Covers.csproj`) with 10 NuGet packages (6 requiring updates, 1 with a security vulnerability). There are no API incompatibilities detected, making this a low-risk upgrade with straightforward steps.

---

## Upgrade Strategy

**Selected Strategy: All-At-Once**

**Justification:**
- Only 1 project in the solution (well below the 5-project threshold)
- Simple dependency structure (no inter-project dependencies)
- Small total codebase (~3,099 LOC)
- No API incompatibilities detected
- All package updates are version bumps to their .NET 10 compatible releases

---

## Dependency Analysis

The solution has a single project with no inter-project dependencies:

- **Phase 1**: `Covers.csproj` (standalone — no project dependencies)

---

## Project-by-Project Plan

### Covers.csproj

- **Current Framework**: net5.0
- **Target Framework**: net10.0
- **Project Kind**: AspNetCore, SDK-style
- **Risk**: 🟢 Low
- **Lines of Code**: 3,099

#### Steps

1. **Update TargetFramework** in `Covers.csproj` from `net5.0` to `net10.0`

2. **Update NuGet packages** (see Package Update Reference below)

3. **Fix any breaking changes** revealed during compilation (none expected based on assessment)

4. **Build and verify** — ensure 0 errors, 0 warnings

---

## Package Update Reference

| Package | Current Version | Target Version | Reason |
| :--- | :---: | :---: | :--- |
| **Magick.NET-Q8-AnyCPU** | 7.22.3 | 14.10.2 | 🔴 Security vulnerability — critical update |
| Microsoft.AspNetCore.SpaServices.Extensions | 5.0.1 | 10.0.3 | Upgrade recommended for .NET 10 compatibility |
| Microsoft.EntityFrameworkCore.Design | 5.0.1 | 10.0.3 | Upgrade recommended for .NET 10 compatibility |
| Microsoft.EntityFrameworkCore.Proxies | 5.0.1 | 10.0.3 | Upgrade recommended for .NET 10 compatibility |
| Microsoft.EntityFrameworkCore.Sqlite | 5.0.1 | 10.0.3 | Upgrade recommended for .NET 10 compatibility |
| Microsoft.EntityFrameworkCore.Tools | 5.0.1 | 10.0.3 | Upgrade recommended for .NET 10 compatibility |
| NSwag.AspNetCore | 13.9.4 | — | ✅ Compatible, no update needed |
| SpotifyAPI.Web | 6.0.0 | — | ✅ Compatible, no update needed |
| SpotifyAPI.Web.Auth | 6.0.0 | — | ✅ Compatible, no update needed |
| TagLibSharp | 2.2.0 | — | ✅ Compatible, no update needed |

> ⚠️ **Security Alert**: Magick.NET-Q8-AnyCPU 7.22.3 has known security vulnerabilities. Upgrade to 14.10.2 is **mandatory**.

---

## Breaking Changes Catalog

Based on assessment, **0 API incompatibilities** were detected. However, the following areas should be monitored during compilation:

1. **Magick.NET v7 → v14**: Major version bump — potential breaking changes in the image processing API (e.g., `MagickGeometry` property type changes from `int` to `uint`). Review all usages in `Controllers/CoverController.cs`, `Services/SpotifyService.cs`, and `BackgroundServices/AlbumScanner.cs`.

2. **Entity Framework Core 5 → 10**: Major version bump — check for API changes in DbContext configuration, migration scripts, and LINQ query patterns.

3. **ASP.NET Core 5 → 10**: Check `Program.cs`/`Startup.cs` for middleware and hosting API changes.

4. **Obsolete APIs**: `.NET 10` removes some APIs deprecated in earlier versions (e.g., `WebClient` is fully obsolete — use `HttpClient` instead).

---

## Testing Strategy

### Per-Project Testing (Phase 1)

After upgrading `Covers.csproj`:

- [ ] Project builds without errors
- [ ] Project builds without warnings
- [ ] Run application and verify core functionality
- [ ] Verify API endpoints respond correctly
- [ ] Verify image processing (Magick.NET) works with new version
- [ ] Verify Spotify API integration works
- [ ] Verify EF Core database operations work

### Full Solution Testing

- [ ] Complete solution builds with 0 errors and 0 warnings
- [ ] No package dependency conflicts
- [ ] No security vulnerabilities remain
- [ ] Application runs end-to-end

---

## Risk Management

### Risks

| Risk | Likelihood | Impact | Mitigation |
| :--- | :---: | :---: | :--- |
| Magick.NET v7→v14 breaking changes | Medium | Medium | Review all Magick.NET usage, fix compilation errors |
| EF Core 5→10 migration issues | Low | Medium | Verify EF Core migrations, test DB operations |
| ASP.NET Core hosting changes | Low | Low | Review Program.cs/Startup.cs patterns |

### Rollback

If the upgrade encounters unresolvable issues, revert to the source branch: `copilot/upgrade-covers-sln-to-dotnet-10-please-work` (original state).

---

## Success Criteria

The upgrade is complete when:

1. `Covers.csproj` targets `net10.0`
2. All 6 package updates applied (including Magick.NET security fix)
3. Solution builds with **0 errors** and **0 warnings**
4. No security vulnerabilities remain in dependencies
5. Application runs and core features function correctly

---

*This plan is ready for Execution stage.*
