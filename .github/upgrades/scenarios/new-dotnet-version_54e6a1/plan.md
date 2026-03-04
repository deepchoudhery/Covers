# Upgrade Plan: Covers .NET 5.0 → .NET 10.0

**Date**: 2026-03-04  
**Source Branch**: copilot/upgrade-covers-to-dotnet-10  
**Target Framework**: net10.0

---

## Executive Summary

The Covers solution contains a single ASP.NET Core web application project targeting .NET 5.0 (end of life). This plan upgrades it to .NET 10.0 (LTS). The project is small (~3,100 LOC), uses an SDK-style project file, and has no inter-project dependencies, making this a low-risk, straightforward upgrade.

Six of the ten NuGet packages require updates, including one with a known security vulnerability (Magick.NET-Q8-AnyCPU). No API incompatibilities were detected by static analysis, though breaking changes from Magick.NET v7 → v14 and EF Core 5 → 10 may surface during compilation.

---

## Upgrade Strategy: All-At-Once

**Rationale**: Only 1 project, small codebase (3,099 LOC), simple flat dependency graph, no test projects to coordinate. All projects can be upgraded simultaneously in a single pass.

---

## Dependency Analysis

The solution contains a single project with no inter-project dependencies:

```
Covers.csproj (standalone)
```

Upgrade order: Covers.csproj (only project)

---

## Project-by-Project Plan

### Covers.csproj

**Current Framework**: net5.0  
**Target Framework**: net10.0  
**Risk Level**: Low  
**Complexity**: Low (0 API issues detected, 3,099 LOC)

#### Step 1: Update TargetFramework

In `Covers.csproj`, change:
```xml
<TargetFramework>net5.0</TargetFramework>
```
to:
```xml
<TargetFramework>net10.0</TargetFramework>
```

#### Step 2: Update Package References

Update all packages with recommended upgrades:

| Package | From | To | Reason |
|---------|------|----|--------|
| Magick.NET-Q8-AnyCPU | 7.22.3 | 14.10.3 | 🔴 Security vulnerability |
| Microsoft.AspNetCore.SpaServices.Extensions | 5.0.1 | 10.0.3 | Framework alignment |
| Microsoft.EntityFrameworkCore.Design | 5.0.1 | 10.0.3 | Framework alignment |
| Microsoft.EntityFrameworkCore.Proxies | 5.0.1 | 10.0.3 | Framework alignment |
| Microsoft.EntityFrameworkCore.Sqlite | 5.0.1 | 10.0.3 | Framework alignment |
| Microsoft.EntityFrameworkCore.Tools | 5.0.1 | 10.0.3 | Framework alignment |

**Compatible packages (no update needed)**:
- NSwag.AspNetCore 13.9.4
- SpotifyAPI.Web 6.0.0
- SpotifyAPI.Web.Auth 6.0.0
- TagLibSharp 2.2.0

#### Step 3: Fix Breaking Changes

Build the project and resolve any compilation errors. Expected areas to check:
- **Magick.NET v7 → v14**: API changes including `MagickGeometry` property types (Width/Height changed from `int` to `uint`)
- **EF Core 5 → 10**: Configuration and API changes (e.g., obsolete methods)
- **ASP.NET Core 5 → 10**: Middleware, `Startup.cs` patterns, or hosting model changes
- **WebClient**: Obsolete in .NET 6+; replace with `HttpClient` if used

#### Step 4: Validate

- Build succeeds with 0 errors and 0 warnings
- Application starts and responds to requests

---

## Package Update Reference

| Package | Current | Target | Projects Affected | Priority |
|---------|---------|--------|-------------------|----------|
| Magick.NET-Q8-AnyCPU | 7.22.3 | 14.10.3 | Covers.csproj | 🔴 Critical (security) |
| Microsoft.AspNetCore.SpaServices.Extensions | 5.0.1 | 10.0.3 | Covers.csproj | High |
| Microsoft.EntityFrameworkCore.Design | 5.0.1 | 10.0.3 | Covers.csproj | High |
| Microsoft.EntityFrameworkCore.Proxies | 5.0.1 | 10.0.3 | Covers.csproj | High |
| Microsoft.EntityFrameworkCore.Sqlite | 5.0.1 | 10.0.3 | Covers.csproj | High |
| Microsoft.EntityFrameworkCore.Tools | 5.0.1 | 10.0.3 | Covers.csproj | High |

---

## Breaking Changes Catalog

### Magick.NET v7 → v14
- `MagickGeometry.Width` and `MagickGeometry.Height` changed from `int` to `uint`
- API method signatures may have changed; review all usages of `MagickImage`, `MagickGeometry`, and related types
- Review all code in `Controllers/CoverController.cs` and any other file using Magick.NET

### EF Core 5 → 10
- Some APIs deprecated in EF Core 6+ may be removed
- Check for any obsolete `DbContextOptions` or configuration patterns

### ASP.NET Core 5 → 10
- `WebClient` is obsolete (SYSLIB0014 warning); replace with `HttpClient`
- `Startup.cs` pattern still supported but `Program.cs` minimal hosting model preferred
- Some `IServiceCollection` extension method signatures may have changed

---

## Testing Strategy

### Validation Steps

- [ ] `dotnet restore` completes without errors
- [ ] `dotnet build` completes with 0 errors and 0 warnings
- [ ] Application starts successfully
- [ ] Core functionality works: album cover fetching, Spotify integration

---

## Risk Management

### Risks

| Risk | Likelihood | Impact | Mitigation |
|------|-----------|--------|------------|
| Magick.NET v14 breaking changes | High | Medium | Review all Magick.NET usages after update |
| EF Core 10 breaking changes | Low | Low | Build and fix compilation errors |
| WebClient obsolescence warnings | Medium | Low | Replace with HttpClient |

### Rollback

Changes are on the upgrade branch `copilot/upgrade-covers-to-dotnet-10`. Rollback by reverting to the source branch.

---

## Success Criteria

- [ ] `Covers.csproj` targets `net10.0`
- [ ] All 6 packages updated to recommended versions
- [ ] Security vulnerability in Magick.NET resolved (updated to 14.10.3)
- [ ] `dotnet build` succeeds with **0 errors and 0 warnings**
- [ ] Application starts and core functionality works

---

*This plan is ready for Execution stage.*
