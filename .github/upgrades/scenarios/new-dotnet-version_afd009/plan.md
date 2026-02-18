# .NET 10 Upgrade Plan for Covers Solution

**Date**: 2026-02-18  
**Planning Mode**: Scenario-Guided

---

## Executive Summary

This plan outlines the upgrade of the Covers solution from .NET 5.0 to .NET 10.0. The solution consists of a single ASP.NET Core project with 10 NuGet packages, 6 of which require updates. The upgrade is classified as **Low Complexity** with no API compatibility issues detected.

**Key Highlights:**
- Single project solution (Covers.csproj)
- 6 package updates required (including 1 security vulnerability fix)
- 4 packages already compatible
- No breaking API changes detected
- Estimated impact: Minimal code changes

---

## Upgrade Strategy

**Selected Approach: All-At-Once Upgrade**

**Rationale:**
- Single project solution
- Simple dependency structure
- Low complexity classification
- No API compatibility issues
- Fast execution path appropriate for small solution

---

## Dependency Analysis

### Project Structure

```
Covers.csproj (ASP.NET Core, .NET 5.0)
├── No project dependencies
└── 10 NuGet packages
```

**Current State:**
- **Target Framework**: net5.0
- **Project Type**: ASP.NET Core Web Application
- **SDK Style**: Yes (modern format)
- **Lines of Code**: 3,099
- **Code Files**: 51

**Proposed State:**
- **Target Framework**: net10.0

---

## Package Update Reference

### Critical Security Update

| Package | Current | Target | Severity | Reason |
|---------|---------|--------|----------|--------|
| Magick.NET-Q8-AnyCPU | 7.22.3 | 14.10.2 | 🔴 Critical | Security vulnerability |

### Framework Package Updates

| Package | Current | Target | Reason |
|---------|---------|--------|--------|
| Microsoft.AspNetCore.SpaServices.Extensions | 5.0.1 | 10.0.3 | Framework alignment |
| Microsoft.EntityFrameworkCore.Design | 5.0.1 | 10.0.3 | Framework alignment |
| Microsoft.EntityFrameworkCore.Proxies | 5.0.1 | 10.0.3 | Framework alignment |
| Microsoft.EntityFrameworkCore.Sqlite | 5.0.1 | 10.0.3 | Framework alignment |
| Microsoft.EntityFrameworkCore.Tools | 5.0.1 | 10.0.3 | Framework alignment |

### Compatible Packages (No Update Required)

| Package | Version | Status |
|---------|---------|--------|
| NSwag.AspNetCore | 13.9.4 | ✅ Compatible |
| SpotifyAPI.Web | 6.0.0 | ✅ Compatible |
| SpotifyAPI.Web.Auth | 6.0.0 | ✅ Compatible |
| TagLibSharp | 2.2.0 | ✅ Compatible |

---

## Breaking Changes Catalog

### Known Breaking Changes

**Magick.NET 7.x → 14.x:**
- `MagickGeometry` Width/Height properties changed from `int` to `uint`
- Requires explicit casts when passing int values
- Affects image resizing operations

**Microsoft.AspNetCore.SpaServices.Extensions:**
- Package may be deprecated in .NET 10
- Monitor for migration path to Vite/modern SPA tooling

**Entity Framework Core 5.x → 10.x:**
- Breaking changes typically minimal for standard scenarios
- Database provider compatibility verified (Sqlite)

---

## Detailed Upgrade Plan

### Phase 1: Project File Updates

**Task 1.1: Update Target Framework**
- File: `Covers.csproj`
- Change: `<TargetFramework>net5.0</TargetFramework>` → `<TargetFramework>net10.0</TargetFramework>`
- Risk: Low
- Validation: Build succeeds

**Task 1.2: Update Package References**

Update all package versions in `Covers.csproj`:

```xml
<PackageReference Include="Magick.NET-Q8-AnyCPU" Version="14.10.2" />
<PackageReference Include="Microsoft.AspNetCore.SpaServices.Extensions" Version="10.0.3" />
<PackageReference Include="Microsoft.EntityFrameworkCore.Design" Version="10.0.3" />
<PackageReference Include="Microsoft.EntityFrameworkCore.Proxies" Version="10.0.3" />
<PackageReference Include="Microsoft.EntityFrameworkCore.Sqlite" Version="10.0.3" />
<PackageReference Include="Microsoft.EntityFrameworkCore.Tools" Version="10.0.3" />
```

- Risk: Low
- Validation: Package restore succeeds

---

### Phase 2: Code Changes

**Task 2.1: Fix Magick.NET Breaking Changes**

Search for MagickGeometry usage and apply uint casts:

**Files to Check:**
- Controllers/CoverController.cs
- Services/SpotifyService.cs
- BackgroundServices/AlbumScanner.cs

**Pattern to Find:**
```csharp
new MagickGeometry(size, size)
new MagickGeometry(width, height)
```

**Fix Pattern:**
```csharp
new MagickGeometry((uint)size, (uint)size)
new MagickGeometry((uint)width, (uint)height)
```

- Risk: Low
- Validation: Build without errors

**Task 2.2: Address Any Obsolete API Warnings**

Check for:
- WebClient usage (obsolete in .NET 6+, use HttpClient)
- Other obsolete APIs flagged during build

- Risk: Low
- Validation: Build without warnings

---

### Phase 3: Dependency Restoration and Build

**Task 3.1: Clean and Restore**
```bash
dotnet clean
dotnet restore
```

- Validation: No restore errors

**Task 3.2: Build Project**
```bash
dotnet build --no-restore
```

- Validation: 0 errors, 0 warnings

---

### Phase 4: Testing and Validation

**Task 4.1: Verify Application Startup**
- Start the application
- Verify no runtime exceptions
- Check all services initialize correctly

**Task 4.2: Functional Testing**
- Test Spotify API integration
- Test album scanning functionality
- Test cover image generation
- Test database operations

**Task 4.3: Security Validation**
- Verify Magick.NET security vulnerability resolved
- Check for any new security warnings

---

## Testing Strategy

### Build Validation
- ✅ Project builds without errors
- ✅ Project builds without warnings
- ✅ All packages restore successfully
- ✅ No dependency conflicts

### Runtime Validation
- ✅ Application starts successfully
- ✅ Database connectivity works
- ✅ Spotify API integration functional
- ✅ Image processing operations work
- ✅ Background services run correctly

### Security Validation
- ✅ No security vulnerabilities in packages
- ✅ No obsolete API warnings

---

## Risk Management

### Risk Assessment

**Covers.csproj - Low Risk**
- Small codebase (3,099 LOC)
- Single project (no dependency complexity)
- No API compatibility issues detected
- Clear migration path for known breaking change

### Mitigation Strategies

1. **Incremental Testing**: Test after each phase
2. **Code Search**: Use grep/search to find all affected patterns
3. **Build Validation**: Ensure zero warnings policy
4. **Rollback Plan**: Git branch allows easy revert if needed

### Rollback Plan

If critical issues arise:
1. Revert changes via Git
2. Return to net5.0 on original branch
3. Investigate specific failure points
4. Re-attempt with targeted fixes

---

## Success Criteria

### Technical Criteria
- ✅ Target framework updated to net10.0
- ✅ All 6 packages updated to specified versions
- ✅ Build succeeds with 0 errors
- ✅ Build succeeds with 0 warnings
- ✅ No package dependency conflicts
- ✅ Security vulnerability in Magick.NET resolved

### Functional Criteria
- ✅ Application starts without errors
- ✅ All existing functionality preserved
- ✅ Database operations work correctly
- ✅ Spotify API integration functional
- ✅ Image processing operations work

---

## Execution Timeline

**Estimated Duration**: 30-45 minutes

| Phase | Tasks | Est. Time |
|-------|-------|-----------|
| Phase 1: Project File Updates | 2 tasks | 5 min |
| Phase 2: Code Changes | 2 tasks | 10-15 min |
| Phase 3: Build | 2 tasks | 5-10 min |
| Phase 4: Testing | 3 tasks | 10-15 min |

---

## Next Steps

1. Review and approve this plan
2. Transition to Execution stage
3. Execute tasks in sequential order
4. Validate at each checkpoint
5. Document results in tasks.md

---

*This plan supports the Execution stage of the .NET 10 upgrade workflow.*
