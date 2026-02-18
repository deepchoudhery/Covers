# .NET 10.0 Upgrade Plan for Covers Solution

**Date**: 2026-02-18  
**Solution**: Covers.sln  
**Current Framework**: .NET 5.0  
**Target Framework**: .NET 10.0  
**Strategy**: All-At-Once

---

## Executive Summary

### Upgrade Scope
- **Total Projects**: 1 (Covers.csproj)
- **Project Type**: ASP.NET Core Web Application with SPA
- **Total Code Files**: 51
- **Total Lines of Code**: 3,099
- **Current Framework**: net5.0
- **Target Framework**: net10.0

### Selected Strategy
**All-At-Once Strategy** - All components upgraded simultaneously in a single operation.

**Rationale**:
- Single project solution (minimal complexity)
- Currently on .NET 5.0 (modern SDK-style project)
- Clear package update path identified
- All packages have .NET 10.0-compatible versions
- Low risk profile (🟢 Low difficulty rating)

### Key Findings from Assessment
- **Package Updates Required**: 6 packages need upgrades
- **Security Issues**: 1 package (Magick.NET-Q8-AnyCPU) has security vulnerabilities
- **API Issues**: 0 binary/source incompatibilities detected
- **Estimated Impact**: Low (0+ LOC requiring changes)

### Success Criteria
1. Project targets .NET 10.0 (net10.0)
2. All package updates applied successfully
3. Solution builds with 0 errors and 0 warnings
4. Application runs correctly
5. No security vulnerabilities remain

---

## Dependency Analysis

### Project Structure
This is a single-project solution with no inter-project dependencies.

```
Covers.csproj (ASP.NET Core Web + React SPA)
├── Backend: ASP.NET Core 5.0 → 10.0
└── Frontend: React (ClientApp) - no framework changes needed
```

### Current State
- **Project Type**: ASP.NET Core Web with SPA Services
- **SDK Style**: Yes (already modernized)
- **Target Framework**: net5.0
- **Package Count**: 10 total packages
- **Security Status**: 1 package with vulnerabilities

---

## Upgrade Strategy: All-At-Once

### Approach
Since this is a single-project solution, we'll perform a coordinated atomic upgrade:

1. **Prerequisite Verification** - Ensure .NET 10 SDK is installed
2. **Atomic Upgrade** - Update project file and all packages simultaneously
3. **Build Validation** - Compile and verify no errors/warnings
4. **Functional Testing** - Verify application runs correctly

### Why All-At-Once?
- Only one project to upgrade
- Clean upgrade path (no complex dependencies)
- All packages compatible with .NET 10.0
- Low risk of breaking changes
- Faster completion vs. incremental approach

---

## Package Update Reference

### Packages Requiring Updates

| Package | Current Version | Target Version | Reason | Priority |
|---------|----------------|----------------|--------|----------|
| Magick.NET-Q8-AnyCPU | 7.22.3 | 14.10.2 | Security vulnerability | 🔴 Critical |
| Microsoft.AspNetCore.SpaServices.Extensions | 5.0.1 | 10.0.3 | Framework alignment | High |
| Microsoft.EntityFrameworkCore.Design | 5.0.1 | 10.0.3 | Framework alignment | High |
| Microsoft.EntityFrameworkCore.Proxies | 5.0.1 | 10.0.3 | Framework alignment | High |
| Microsoft.EntityFrameworkCore.Sqlite | 5.0.1 | 10.0.3 | Framework alignment | High |
| Microsoft.EntityFrameworkCore.Tools | 5.0.1 | 10.0.3 | Framework alignment | High |

### Packages Remaining Compatible (No Update Required)

| Package | Version | Status |
|---------|---------|--------|
| NSwag.AspNetCore | 13.9.4 | ✅ Compatible with .NET 10.0 |
| SpotifyAPI.Web | 6.0.0 | ✅ Compatible with .NET 10.0 |
| SpotifyAPI.Web.Auth | 6.0.0 | ✅ Compatible with .NET 10.0 |
| TagLibSharp | 2.2.0 | ✅ Compatible with .NET 10.0 |

### Package Update Details

#### Critical: Magick.NET-Q8-AnyCPU
- **Current**: 7.22.3
- **Target**: 14.10.2
- **Reason**: Contains known security vulnerabilities
- **Breaking Changes**: Major version jump (7.x → 14.x)
  - MagickGeometry Width/Height properties changed from `int` to `uint`
  - API modernization may affect image processing code
- **Files to Review**: Controllers/CoverController.cs, Services/SpotifyService.cs, BackgroundServices/AlbumScanner.cs

#### Entity Framework Core Packages
All EF Core packages upgrade from 5.0.1 → 10.0.3:
- **Breaking Changes**: Minimal for basic usage
- **Benefit**: Performance improvements, new features
- **Risk**: Low (well-established upgrade path)

#### ASP.NET Core SPA Services
- **Current**: 5.0.1
- **Target**: 10.0.3
- **Note**: Package is deprecated in .NET 6+, but migration to new approach not required for basic functionality
- **Risk**: Low for this upgrade, but consider future migration

---

## Breaking Changes Catalog

### .NET 5.0 → .NET 10.0 Framework Changes

#### ASP.NET Core
- Minimal breaking changes for basic applications
- Middleware registration patterns unchanged
- Dependency injection patterns unchanged

#### Entity Framework Core
- Query behavior improvements (more strict nullability)
- Some performance optimizations may change query execution
- Migration generation unchanged for basic scenarios

### Package-Specific Breaking Changes

#### Magick.NET 7.x → 14.x
**Known Breaking Changes**:
1. **MagickGeometry Property Types**
   - `Width` and `Height` changed from `int` to `uint`
   - **Impact**: May cause compilation errors
   - **Fix**: Cast or update variable types

2. **API Modernization**
   - Some deprecated methods removed
   - Constructor signatures may have changed
   - **Impact**: Code using deprecated APIs will break
   - **Fix**: Update to new API patterns

**Files Likely Affected**:
- Controllers/CoverController.cs (album cover processing)
- Services/SpotifyService.cs (image handling)
- BackgroundServices/AlbumScanner.cs (background image processing)

---

## Implementation Timeline

### Phase 0: Prerequisite Verification
**Duration**: 5 minutes

**Tasks**:
1. Verify .NET 10.0 SDK installation
   - Run: `dotnet --list-sdks`
   - Confirm: 10.0.x SDK is available
2. Check global.json (if exists) for SDK version constraints
   - Update if necessary to allow .NET 10.0

**Deliverables**: Environment ready for .NET 10.0 development

### Phase 1: Atomic Upgrade
**Duration**: 30-60 minutes

**Operations** (performed as single coordinated batch):

1. **Update Project File** (Covers.csproj)
   - Change `<TargetFramework>net5.0</TargetFramework>` to `<TargetFramework>net10.0</TargetFramework>`

2. **Update Package References**
   - Magick.NET-Q8-AnyCPU: 7.22.3 → 14.10.2
   - Microsoft.AspNetCore.SpaServices.Extensions: 5.0.1 → 10.0.3
   - Microsoft.EntityFrameworkCore.Design: 5.0.1 → 10.0.3
   - Microsoft.EntityFrameworkCore.Proxies: 5.0.1 → 10.0.3
   - Microsoft.EntityFrameworkCore.Sqlite: 5.0.1 → 10.0.3
   - Microsoft.EntityFrameworkCore.Tools: 5.0.1 → 10.0.3

3. **Restore Dependencies**
   - Run: `dotnet restore`
   - Verify: All packages downloaded successfully

4. **Build Solution**
   - Run: `dotnet build --no-restore`
   - Identify: Compilation errors (if any)

5. **Fix Compilation Errors**
   - Address Magick.NET breaking changes
     - Update `int` to `uint` for Width/Height properties
     - Update any deprecated API usage
   - Address any other framework-related issues

6. **Rebuild and Verify**
   - Run: `dotnet build --no-restore`
   - Confirm: 0 errors, 0 warnings

**Deliverables**: 
- Solution builds successfully with .NET 10.0
- No compilation errors or warnings
- All packages updated

### Phase 2: Validation
**Duration**: 15-30 minutes

**Operations**:

1. **Functional Testing**
   - Run application: `dotnet run`
   - Verify: Application starts without errors
   - Test: Basic functionality (cover image processing, Spotify integration)
   - Test: Album scanning background service

2. **Database Verification**
   - Verify: EF Core migrations compatibility
   - Test: Database operations (CRUD)

3. **Integration Points**
   - Test: Spotify API integration
   - Test: Image processing with Magick.NET
   - Test: File tagging with TagLibSharp

**Deliverables**:
- Application runs successfully
- All integrations working
- No runtime errors

### Phase 3: Final Verification
**Duration**: 10 minutes

**Operations**:

1. **Security Audit**
   - Confirm: Magick.NET updated to secure version (14.10.2)
   - Verify: No remaining security vulnerabilities

2. **Build Quality Check**
   - Run: `dotnet build`
   - Confirm: 0 errors, 0 warnings

3. **Documentation**
   - Update: README.md with .NET 10.0 requirement (if needed)
   - Note: Any API changes for future developers

**Deliverables**:
- Clean build
- No security issues
- Documentation updated

---

## Risk Management

### Risk Assessment

#### Low Risk Items ✅
- **Framework Upgrade**: .NET 5.0 → 10.0 (well-established path)
- **EF Core Updates**: Minor version upgrade with good compatibility
- **Project Structure**: Single project, no complex dependencies

#### Medium Risk Items ⚠️
- **Magick.NET Major Upgrade**: 7.x → 14.x (major version jump)
  - **Mitigation**: Review and test all image processing code
  - **Rollback**: Version 7.22.3 known to work

#### High Risk Items 🔴
- **Security Vulnerabilities**: Current Magick.NET version has CVEs
  - **Mitigation**: Prioritize upgrade, test thoroughly
  - **Impact**: Must upgrade to maintain security posture

### Risk Mitigation Strategies

#### Before Starting
1. ✅ Ensure working on upgrade branch (copilot/upgrade-covers-sln-to-dotnet-10-again)
2. ✅ Commit any pending changes
3. ✅ Verify .NET 10.0 SDK installed

#### During Upgrade
1. **Incremental Verification**: Build after each change group
2. **Error Documentation**: Note all compilation errors for analysis
3. **API Research**: Consult Magick.NET migration guide for breaking changes

#### Testing Strategy
1. **Compilation**: Must build with 0 errors, 0 warnings
2. **Functional**: Core features must work (cover processing, scanning)
3. **Integration**: External APIs must connect properly
4. **Performance**: No significant regression in processing time

### Rollback Plan

If upgrade fails or causes critical issues:

1. **Immediate Rollback**
   ```bash
   git checkout main
   git branch -D copilot/upgrade-covers-sln-to-dotnet-10-again
   ```

2. **Partial Rollback** (if specific package causes issues)
   - Revert specific package to previous version
   - Document incompatibility
   - Research alternative solutions

3. **Staged Approach** (if all-at-once fails)
   - Upgrade framework first, keep packages at 5.0.1
   - Upgrade packages incrementally
   - Test after each package update

---

## Testing Strategy

### Multi-Level Testing

#### Level 1: Compilation
- **Requirement**: Solution must build
- **Acceptance**: 0 errors, 0 warnings
- **Command**: `dotnet build --no-restore`

#### Level 2: Unit/Integration Tests
- **Note**: Assessment doesn't indicate existing test projects
- **Action**: Manual functional testing required

#### Level 3: Functional Testing
**Critical Scenarios**:
1. Application startup and initialization
2. Cover image download and processing
3. Spotify API authentication and data retrieval
4. Album scanning background service
5. Database operations (SQLite)
6. File tagging operations

**Test Cases**:
- ✅ Application starts without errors
- ✅ Can process album cover images
- ✅ Spotify integration works (auth + API calls)
- ✅ Background scanner processes albums
- ✅ Database queries execute correctly
- ✅ Tag reading/writing functions properly

#### Level 4: Performance Validation
- **Baseline**: Note current performance metrics
- **Post-Upgrade**: Compare processing times
- **Threshold**: No more than 10% regression acceptable

---

## Source Control

### Branch Strategy
- **Source Branch**: copilot/upgrade-covers-sln-to-dotnet-10-again (current)
- **Commit Strategy**: Single atomic commit preferred
  - All changes in one commit for easy rollback
  - Clear commit message: ".NET 10.0 upgrade with package updates"

### Commit Structure

**Preferred**: Single Commit
```
feat: Upgrade to .NET 10.0 with package updates

- Update TargetFramework: net5.0 → net10.0
- Update Magick.NET-Q8-AnyCPU: 7.22.3 → 14.10.2 (security fix)
- Update Microsoft.AspNetCore.SpaServices.Extensions: 5.0.1 → 10.0.3
- Update EF Core packages: 5.0.1 → 10.0.3
- Fix Magick.NET breaking changes (int → uint)
- Verify: 0 errors, 0 warnings
```

**Alternative**: Staged Commits (if needed)
1. Project file updates
2. Package updates
3. Code fixes for breaking changes

---

## Detailed Execution Steps

### Step 1: Verify Prerequisites
1. Check .NET 10.0 SDK installation
   ```bash
   dotnet --list-sdks
   ```
   Expected: 10.0.x in list

2. Check for global.json constraints
   ```bash
   cat global.json 2>/dev/null || echo "No global.json found"
   ```
   If exists: Update to allow .NET 10.0

### Step 2: Update Project File
Edit `Covers.csproj`:

**Change**:
```xml
<TargetFramework>net5.0</TargetFramework>
```

**To**:
```xml
<TargetFramework>net10.0</TargetFramework>
```

### Step 3: Update Package References
Edit `Covers.csproj`, update all PackageReference versions:

**Current**:
```xml
<PackageReference Include="Magick.NET-Q8-AnyCPU" Version="7.22.3" />
<PackageReference Include="Microsoft.AspNetCore.SpaServices.Extensions" Version="5.0.1" />
<PackageReference Include="Microsoft.EntityFrameworkCore.Design" Version="5.0.1">
<PackageReference Include="Microsoft.EntityFrameworkCore.Proxies" Version="5.0.1" />
<PackageReference Include="Microsoft.EntityFrameworkCore.Sqlite" Version="5.0.1" />
<PackageReference Include="Microsoft.EntityFrameworkCore.Tools" Version="5.0.1">
```

**Update To**:
```xml
<PackageReference Include="Magick.NET-Q8-AnyCPU" Version="14.10.2" />
<PackageReference Include="Microsoft.AspNetCore.SpaServices.Extensions" Version="10.0.3" />
<PackageReference Include="Microsoft.EntityFrameworkCore.Design" Version="10.0.3">
<PackageReference Include="Microsoft.EntityFrameworkCore.Proxies" Version="10.0.3" />
<PackageReference Include="Microsoft.EntityFrameworkCore.Sqlite" Version="10.0.3" />
<PackageReference Include="Microsoft.EntityFrameworkCore.Tools" Version="10.0.3">
```

### Step 4: Restore and Build
```bash
dotnet restore
dotnet build --no-restore
```

### Step 5: Fix Compilation Errors

**Expected Errors** (Magick.NET):

Error locations:
- Controllers/CoverController.cs
- Services/SpotifyService.cs
- BackgroundServices/AlbumScanner.cs

**Common Pattern**:
```csharp
// Before (error: cannot convert uint to int)
int width = geometry.Width;
int height = geometry.Height;

// After (correct)
uint width = geometry.Width;
uint height = geometry.Height;
```

**Or with casting**:
```csharp
// If int is required downstream
int width = (int)geometry.Width;
int height = (int)geometry.Height;
```

### Step 6: Rebuild and Verify
```bash
dotnet build --no-restore
```

**Success Criteria**:
- Build succeeded
- 0 Error(s)
- 0 Warning(s)

### Step 7: Run Application
```bash
dotnet run
```

**Verify**:
- Application starts
- No startup errors
- Web UI loads
- Background services start

### Step 8: Test Core Functionality
Manual test scenarios:
1. Access application at localhost:5000 (or configured port)
2. Trigger album scan
3. Verify cover image processing
4. Check database operations
5. Test Spotify integration (if configured)

---

## Breaking Changes Reference

### .NET 5.0 → .NET 10.0

#### Removed APIs
- None expected to affect this application

#### Behavioral Changes
- String comparison may have subtle differences
- JSON serialization defaults may differ
- Minimal impact expected for this codebase

#### ASP.NET Core Changes
- Middleware registration: No changes required
- Dependency injection: No changes required
- SPA Services: Package deprecated but still functional

#### Entity Framework Core Changes
- Query translation improvements
- Better null handling
- Performance optimizations
- No breaking changes for basic CRUD operations

### Magick.NET 7.x → 14.x

#### Type Changes
**MagickGeometry Properties**:
- `Width`: `int` → `uint`
- `Height`: `int` → `uint`

**Impact**: Compilation errors where `int` expected

**Solution**:
```csharp
// Option 1: Change variable type
uint width = geometry.Width;

// Option 2: Explicit cast (if int needed)
int width = (int)geometry.Width;

// Option 3: Use checked cast with validation
int width = checked((int)geometry.Width);
```

#### API Removals
- Check Magick.NET changelog for deprecated methods
- Update to equivalent modern APIs
- Consult: https://github.com/dlemstra/Magick.NET/releases

---

## Success Criteria

### Technical Requirements
- [ ] Project file targets net10.0
- [ ] All 6 packages updated to target versions
- [ ] `dotnet build` succeeds with 0 errors
- [ ] `dotnet build` completes with 0 warnings
- [ ] Application runs without startup errors
- [ ] No security vulnerabilities reported

### Functional Requirements
- [ ] Cover image processing works
- [ ] Spotify API integration functional
- [ ] Album scanning service operational
- [ ] Database operations successful
- [ ] File tagging features working

### Quality Requirements
- [ ] No code smell introduced
- [ ] No performance degradation
- [ ] Clean git history
- [ ] Documentation updated (if needed)

---

## Post-Upgrade Actions

### Immediate
1. Commit changes with descriptive message
2. Push upgrade branch to origin
3. Create pull request for review
4. Run any CI/CD pipelines

### Near-Term
1. Monitor application in deployment
2. Watch for runtime errors or exceptions
3. Track performance metrics
4. Gather user feedback

### Future Considerations
1. **SPA Services Migration**: Microsoft.AspNetCore.SpaServices.Extensions is deprecated
   - Consider migrating to SPA proxy approach
   - Not urgent, but plan for future

2. **Test Coverage**: Consider adding automated tests
   - Unit tests for services
   - Integration tests for controllers
   - Helps with future upgrades

3. **Keep Dependencies Updated**: Stay current with patch releases
   - Regular `dotnet list package --outdated`
   - Security updates as released

---

## Appendix

### Reference Documentation
- .NET 10 Release Notes: https://learn.microsoft.com/en-us/dotnet/core/whats-new/dotnet-10
- Magick.NET Releases: https://github.com/dlemstra/Magick.NET/releases
- EF Core 10 What's New: https://learn.microsoft.com/en-us/ef/core/what-is-new/ef-core-10.0/whatsnew
- ASP.NET Core 10 Changes: https://learn.microsoft.com/en-us/aspnet/core/release-notes/aspnetcore-10.0

### Package Version Matrix

| Package | .NET 5.0 | .NET 10.0 | Change |
|---------|----------|-----------|--------|
| Magick.NET-Q8-AnyCPU | 7.22.3 | 14.10.2 | Major upgrade |
| Microsoft.AspNetCore.SpaServices.Extensions | 5.0.1 | 10.0.3 | Minor upgrade |
| Microsoft.EntityFrameworkCore.* | 5.0.1 | 10.0.3 | Minor upgrade |
| NSwag.AspNetCore | 13.9.4 | 13.9.4 | No change |
| SpotifyAPI.Web | 6.0.0 | 6.0.0 | No change |
| SpotifyAPI.Web.Auth | 6.0.0 | 6.0.0 | No change |
| TagLibSharp | 2.2.0 | 2.2.0 | No change |

---

**Plan Version**: 1.0  
**Last Updated**: 2026-02-18  
**Status**: Ready for Execution

