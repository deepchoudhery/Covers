# .NET 10 Upgrade Plan for Covers Solution

## Executive Summary

### Selected Strategy
**All-At-Once Strategy** - All projects upgraded simultaneously in single operation.

**Rationale**: 
- Single project solution (simple structure)
- Currently on .NET 5.0
- Clear dependency structure
- All packages have target framework versions available
- Low complexity (3,099 lines of code)

### Upgrade Scope
- **Projects to Upgrade**: 1 project (Covers.csproj)
- **Current Framework**: net5.0
- **Target Framework**: net10.0
- **Packages Requiring Update**: 6 packages
- **Compatible Packages**: 4 packages (no update needed)
- **Security Vulnerabilities**: 1 package (Magick.NET-Q8-AnyCPU)

### Success Criteria
1. All project files updated to target net10.0
2. All package updates applied
3. Solution builds with 0 errors
4. Solution builds with 0 warnings
5. No security vulnerabilities remain

---

## Implementation Timeline

### Phase 0: Preparation
- Verify .NET 10 SDK is installed
- Review current repository state
- Ensure clean working directory

### Phase 1: Atomic Upgrade
**Operations** (performed as single coordinated batch):
- Update project file to target framework net10.0
- Update all package references to versions compatible with net10.0
- Restore dependencies
- Build solution to identify compilation errors
- Fix all compilation errors related to framework/package upgrades
- Rebuild solution to verify all fixes

**Deliverables**: 
- Solution builds with 0 errors
- Solution builds with 0 warnings

### Phase 2: Validation
**Operations**:
- Verify no security vulnerabilities remain
- Verify all packages are at recommended versions
- Final build verification

**Deliverables**: 
- Clean build with no errors or warnings
- All security vulnerabilities resolved

---

## Detailed Execution Steps

### Step 1: Update Project File
Update TargetFramework property in Covers.csproj from net5.0 to net10.0:

**Project**: Covers.csproj
- Current: `<TargetFramework>net5.0</TargetFramework>`
- Target: `<TargetFramework>net10.0</TargetFramework>`

### Step 2: Update Package References
Update all NuGet package references to versions compatible with net10.0.

#### Critical Security Update
**Magick.NET-Q8-AnyCPU**: 7.22.3 → 14.10.2
- **Reason**: Security vulnerability in current version
- **Priority**: CRITICAL - Must be updated

#### Entity Framework Core Packages
All Entity Framework Core packages upgraded to version 10.0.3:
- **Microsoft.EntityFrameworkCore.Design**: 5.0.1 → 10.0.3
- **Microsoft.EntityFrameworkCore.Proxies**: 5.0.1 → 10.0.3
- **Microsoft.EntityFrameworkCore.Sqlite**: 5.0.1 → 10.0.3
- **Microsoft.EntityFrameworkCore.Tools**: 5.0.1 → 10.0.3

**Reason**: Framework compatibility and feature updates

#### ASP.NET Core Packages
**Microsoft.AspNetCore.SpaServices.Extensions**: 5.0.1 → 10.0.3
- **Reason**: Framework compatibility

#### Packages Remaining Unchanged (Already Compatible)
- **NSwag.AspNetCore**: 13.9.4 (Compatible)
- **SpotifyAPI.Web**: 6.0.0 (Compatible)
- **SpotifyAPI.Web.Auth**: 6.0.0 (Compatible)
- **TagLibSharp**: 2.2.0 (Compatible)

### Step 3: Restore Dependencies
Execute `dotnet restore` to download updated packages and verify dependency resolution.

### Step 4: Build and Address Compilation Errors
Build the solution and address any compilation errors that arise from:
- Framework API changes between .NET 5.0 and .NET 10.0
- Package API changes (especially Magick.NET major version upgrade)
- Deprecated API usage warnings

Expected areas requiring attention based on repository memories:
- **Magick.NET API changes**: The upgrade from v7 to v14 may require updating code that uses MagickGeometry or other imaging APIs
- **WebClient usage**: If present, replace with HttpClient to avoid obsolete API warnings (SYSLIB0014)

### Step 5: Verify Build Success
Rebuild solution to confirm:
- 0 compilation errors
- 0 warnings
- All packages restored successfully

---

## Package Update Reference

| Package | Current Version | Target Version | Update Reason | Priority |
|---------|----------------|----------------|---------------|----------|
| Magick.NET-Q8-AnyCPU | 7.22.3 | 14.10.2 | Security vulnerability | CRITICAL |
| Microsoft.AspNetCore.SpaServices.Extensions | 5.0.1 | 10.0.3 | Framework compatibility | High |
| Microsoft.EntityFrameworkCore.Design | 5.0.1 | 10.0.3 | Framework compatibility | High |
| Microsoft.EntityFrameworkCore.Proxies | 5.0.1 | 10.0.3 | Framework compatibility | High |
| Microsoft.EntityFrameworkCore.Sqlite | 5.0.1 | 10.0.3 | Framework compatibility | High |
| Microsoft.EntityFrameworkCore.Tools | 5.0.1 | 10.0.3 | Framework compatibility | High |
| NSwag.AspNetCore | 13.9.4 | (unchanged) | Already compatible | N/A |
| SpotifyAPI.Web | 6.0.0 | (unchanged) | Already compatible | N/A |
| SpotifyAPI.Web.Auth | 6.0.0 | (unchanged) | Already compatible | N/A |
| TagLibSharp | 2.2.0 | (unchanged) | Already compatible | N/A |

---

## Breaking Changes Catalog

### .NET 5.0 to .NET 10.0 Framework Changes

#### Potential Breaking Changes to Watch For:

1. **ASP.NET Core Changes**
   - Startup.cs patterns may have evolved
   - Middleware registration patterns
   - Configuration system changes
   - Minimal API patterns (if adopting)

2. **Entity Framework Core Changes**
   - EF Core 5.0 → 10.0 may include query behavior changes
   - Migration generation differences
   - Database provider compatibility
   - Lazy loading proxy patterns

3. **Obsolete API Removals**
   - APIs deprecated in .NET 5/6/7/8/9 may be removed in .NET 10
   - Compiler warnings will identify specific cases
   - Replace WebClient with HttpClient if present

### Package-Specific Breaking Changes

#### Magick.NET v7 → v14 (Major Version Upgrade)

**Critical**: This is a major version jump (7 major versions) and may contain significant breaking changes.

Based on repository memories from previous upgrade:
- **MagickGeometry property changes**: Width/Height properties changed from `int` to `uint`
  - Affected files (from previous .NET 10 upgrade memory):
    - Controllers/CoverController.cs:75
    - Services/SpotifyService.cs:187
    - BackgroundServices/AlbumScanner.cs:412
  - Fix: Cast int values to uint when setting these properties

**Expected Changes**:
- Property type changes (int → uint for dimensions)
- Method signature changes
- Possible API reorganization
- Performance improvements
- New features available

**Mitigation**:
- Carefully review compilation errors in files using Magick.NET
- Refer to Magick.NET changelog for v8-v14 breaking changes
- Test image processing functionality thoroughly
- Update type usage as indicated by compiler errors

#### Entity Framework Core 5.0 → 10.0

**Expected Changes**:
- Query behavior refinements
- New required configurations for some scenarios
- Database provider updates
- Performance improvements

**Mitigation**:
- Review EF Core 6-10 breaking changes documentation
- Test database queries and migrations
- Verify lazy loading proxy behavior

---

## Risk Assessment

### Project Risk Analysis

**Covers.csproj**: 🟢 Low Risk
- **Codebase Size**: 3,099 LOC (small)
- **Current Framework**: net5.0 (modern framework, easier path to net10.0)
- **Project Type**: ASP.NET Core (well-supported upgrade path)
- **Package Issues**: 6 updates required, all have clear versions
- **API Issues**: 0 binary/source incompatibilities detected
- **Security**: 1 vulnerability (will be resolved)

### Overall Solution Risk: 🟢 Low

**Factors Contributing to Low Risk**:
- Single project (simple structure)
- Small codebase
- SDK-style project (modern format)
- All packages have clear upgrade paths
- No complex inter-project dependencies
- Modern starting framework (net5.0)

**Factors Requiring Attention**:
- Magick.NET major version upgrade (v7 → v14) - largest package change
- Multiple framework versions skipped (.NET 5 → 10)
- Testing image processing functionality

### Risk Mitigation Strategies

1. **For Magick.NET Upgrade**:
   - Address all compilation errors methodically
   - Apply known fixes from repository memories (uint type changes)
   - Test image processing scenarios
   - Review Magick.NET release notes for v8-v14

2. **For Framework Upgrade**:
   - Review .NET 6, 7, 8, 9, and 10 breaking changes documentation
   - Pay attention to ASP.NET Core middleware and configuration changes
   - Test EF Core query patterns

3. **For Build Warnings**:
   - Treat warnings as errors during upgrade
   - Replace obsolete APIs (e.g., WebClient → HttpClient)
   - Ensure clean build before completion

---

## Validation Strategy

### Build Validation
- ✅ Solution builds without errors
- ✅ Solution builds without warnings
- ✅ All packages restore successfully
- ✅ No dependency conflicts

### Package Validation
- ✅ All recommended package updates applied
- ✅ Security vulnerability in Magick.NET resolved
- ✅ Package versions compatible with net10.0

### Functional Validation
- ✅ Application compiles successfully
- ✅ No obsolete API warnings
- ✅ All type conversions correct (especially Magick.NET uint types)

---

## Source Control Strategy

**Approach**: Single commit for entire upgrade

**Rationale**: 
- All-At-Once strategy means atomic operation
- Single project makes tracking simple
- All changes are tightly coupled

**Commit Structure**:
- Update project file (TargetFramework)
- Update all package references
- Fix compilation errors from framework/package changes
- Single commit message: "chore: upgrade to .NET 10.0 with package updates"

---

## Project-by-Project Specifications

### Project: Covers.csproj

**Current State**:
- **Framework**: net5.0
- **Type**: ASP.NET Core application
- **SDK Style**: Yes (modern project format)
- **LOC**: 3,099
- **Files**: 51 code files
- **Package Count**: 10 packages (6 need updates)

**Target State**:
- **Framework**: net10.0
- **Updated Packages**: 6 packages at net10.0-compatible versions
- **Unchanged Packages**: 4 packages (already compatible)

**Upgrade Steps**:

1. **Update TargetFramework**:
   ```xml
   <TargetFramework>net10.0</TargetFramework>
   ```

2. **Update Package References**:
   - Magick.NET-Q8-AnyCPU: 7.22.3 → 14.10.2
   - Microsoft.AspNetCore.SpaServices.Extensions: 5.0.1 → 10.0.3
   - Microsoft.EntityFrameworkCore.Design: 5.0.1 → 10.0.3
   - Microsoft.EntityFrameworkCore.Proxies: 5.0.1 → 10.0.3
   - Microsoft.EntityFrameworkCore.Sqlite: 5.0.1 → 10.0.3
   - Microsoft.EntityFrameworkCore.Tools: 5.0.1 → 10.0.3

3. **Expected Breaking Changes**:
   - Magick.NET: int → uint for MagickGeometry Width/Height
     - Fix locations: Controllers/CoverController.cs, Services/SpotifyService.cs, BackgroundServices/AlbumScanner.cs
   - Potential WebClient → HttpClient replacement if used
   - ASP.NET Core configuration or middleware changes
   - EF Core query or proxy behavior changes

4. **Validation Criteria**:
   - ✅ Project builds without errors
   - ✅ Project builds without warnings
   - ✅ All packages compatible with net10.0
   - ✅ No security vulnerabilities
   - ✅ Image processing functionality works (Magick.NET)
   - ✅ Database access works (EF Core)

**Files Requiring Attention** (based on known Magick.NET changes):
- `Controllers/CoverController.cs` - MagickGeometry usage
- `Services/SpotifyService.cs` - MagickGeometry usage
- `BackgroundServices/AlbumScanner.cs` - MagickGeometry usage

---

## Additional Considerations

### Testing Recommendations
While the solution doesn't appear to have test projects in the current structure, manual testing should focus on:
- Album cover image processing
- Spotify API integration
- Background album scanning service
- Database persistence operations
- Web API endpoints

### Performance Expectations
.NET 10.0 should provide:
- Improved runtime performance
- Better memory efficiency
- Enhanced garbage collection
- ASP.NET Core performance improvements

### Post-Upgrade Opportunities
After successful upgrade, consider:
- Adopting .NET 10 specific features
- Leveraging performance improvements
- Updating to C# 13 language features
- Reviewing new ASP.NET Core capabilities

---

## Appendix: Assessment Summary

### Project Metrics
| Metric | Value |
|--------|-------|
| Total Projects | 1 |
| Total Packages | 10 |
| Packages Needing Update | 6 |
| Total Code Files | 51 |
| Total Lines of Code | 3,099 |
| Estimated LOC Impact | 0+ (minimal) |

### Package Status Distribution
| Status | Count | Percentage |
|--------|-------|-----------|
| ✅ Compatible | 4 | 40% |
| 🔄 Update Recommended | 6 | 60% |
| ⚠️ Security Issue | 1 | 10% |

### Assessment Files
- Full analysis: `.github/upgrades/scenarios/new-dotnet-version_90e299/assessment.md`
- Data export: `.github/upgrades/scenarios/new-dotnet-version_90e299/assessment.json`
- CSV format: `.github/upgrades/scenarios/new-dotnet-version_90e299/assessment.csv`

---

*This plan provides the complete specification for upgrading Covers solution from .NET 5.0 to .NET 10.0 using the All-At-Once strategy. The Execution stage will implement these changes systematically.*
