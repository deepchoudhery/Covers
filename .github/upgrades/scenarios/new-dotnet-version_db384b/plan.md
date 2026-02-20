# .NET 10.0 Upgrade Plan for Covers Solution

**Date**: 2025-01-26  
**Strategy**: All-At-Once  
**Target Framework**: .NET 10.0 (Long Term Support)

---

## Executive Summary

### Selected Strategy
**All-At-Once Strategy** - Single project upgraded in one coordinated operation.

**Rationale**: 
- Single project solution (Covers.csproj)
- Currently on .NET 5.0
- Clear, straightforward structure (ASP.NET Core SPA application)
- All packages have compatible versions or upgrade paths available
- Low risk profile with comprehensive package ecosystem support

### Current State
- **Project**: Covers.csproj
- **Current Framework**: net5.0
- **Target Framework**: net10.0
- **Project Type**: ASP.NET Core Web Application with React SPA
- **Lines of Code**: 3,099
- **Total NuGet Packages**: 10
- **Packages Requiring Update**: 6
- **Security Vulnerabilities**: 1 (Magick.NET-Q8-AnyCPU)

### Upgrade Scope
- Framework upgrade: net5.0 → net10.0
- Major version jump: 5 versions (5.0 → 6.0 → 7.0 → 8.0 → 9.0 → 10.0)
- Package updates: 6 packages
- Known breaking changes from user context:
  - WebClient obsolescence (replaced with HttpClient via DI)
  - Magick.NET v14 API changes (Width/Height: int → uint)

---

## Risk Assessment

### Overall Risk Level: 🟡 Medium

**Medium Risk Factors:**
- Major version jump (5 versions)
- Security vulnerability in Magick.NET requires major version upgrade (7.22.3 → 14.10.2)
- Known breaking changes in Magick.NET API
- ASP.NET Core hosting model evolution over 5 versions
- Entity Framework Core updates across 5 versions
- SpaServices.Extensions deprecated (will require alternative approach)

**Mitigating Factors:**
- Single project (simple structure)
- Good package ecosystem support
- Known issues already documented
- SDK-style project (modern format)
- Comprehensive assessment completed

### Risk Mitigation Strategies

1. **Breaking Changes**
   - Apply known fixes upfront (WebClient → HttpClient, Magick.NET casts)
   - Test incrementally after each category of changes
   - Maintain detailed change log

2. **Package Updates**
   - Address security vulnerability (Magick.NET) immediately
   - Update framework packages together atomically
   - Verify compatibility after updates

3. **Testing**
   - Build validation after each phase
   - Manual testing of core functionality
   - SPA build validation

4. **Rollback Plan**
   - Working on dedicated branch: copilot/upgrade-covers-sln-dotnet-10
   - Can revert to previous commit if needed
   - Original source branch preserved

---

## Implementation Timeline

### Phase 0: Preparation
**Duration**: 5 minutes  
**Operations**:
- Verify .NET 10 SDK installation
- Ensure working tree is clean (on upgrade branch)
- Document current build baseline

**Success Criteria**:
- [ ] .NET 10 SDK confirmed installed
- [ ] Working on correct branch (copilot/upgrade-covers-sln-dotnet-10)
- [ ] No pending uncommitted changes

### Phase 1: Atomic Upgrade
**Duration**: 15-20 minutes  
**Operations** (performed as single coordinated batch):
- Update project file TargetFramework to net10.0
- Update all package references to target versions
- Apply known breaking change fixes
- Build and verify compilation

**Success Criteria**:
- [ ] Project file updated to net10.0
- [ ] All 6 packages updated to target versions
- [ ] Solution builds with 0 errors
- [ ] Solution builds with 0 warnings

**Deliverables**: 
- Updated Covers.csproj with net10.0 target
- All packages at compatible versions
- Clean build (no errors or warnings)

### Phase 2: Breaking Changes Resolution
**Duration**: 20-30 minutes  
**Operations**:
- Fix SpaServices.Extensions deprecation
- Fix WebClient obsolescence (if present)
- Fix Magick.NET API changes (int → uint casts)
- Address any additional compilation errors
- Iteratively build until clean

**Success Criteria**:
- [ ] All breaking changes resolved
- [ ] Solution builds with 0 errors
- [ ] Solution builds with 0 warnings
- [ ] Code compiles cleanly

**Deliverables**:
- All code updated for .NET 10 compatibility
- Clean compilation

### Phase 3: Verification & Testing
**Duration**: 10-15 minutes  
**Operations**:
- Build solution in Release configuration
- Verify SPA builds correctly (npm build in ClientApp)
- Manual smoke testing of critical paths
- Verify no security vulnerabilities remain

**Success Criteria**:
- [ ] Release build succeeds
- [ ] SPA builds without errors
- [ ] Application runs successfully
- [ ] Core features functional
- [ ] No security alerts

**Deliverables**:
- Verified working application on .NET 10

### Phase 4: Finalization
**Duration**: 5 minutes  
**Operations**:
- Final build validation
- Commit all changes with descriptive message
- Update tasks.md with completion status

**Success Criteria**:
- [ ] Final clean build confirmed
- [ ] All changes committed
- [ ] tasks.md updated

**Deliverables**:
- Committed upgrade on branch
- Updated task tracking

**Total Estimated Duration**: 55-75 minutes

---

## Detailed Execution Steps

### Step 1: Verify Prerequisites

**SDK Installation**:
- Verify .NET 10 SDK is installed
- Command: `dotnet --list-sdks`
- Required: 10.0.x SDK present

**Branch Verification**:
- Confirm on branch: copilot/upgrade-covers-sln-dotnet-10
- Ensure clean working tree

### Step 2: Update Project File

**File**: `D:\a\Covers\Covers\Covers.csproj`

**Changes**:
```xml
<!-- Change from: -->
<TargetFramework>net5.0</TargetFramework>

<!-- To: -->
<TargetFramework>net10.0</TargetFramework>
```

### Step 3: Update Package References

See §Package Update Reference for complete details.

**Packages to Update** (6 total):
1. Magick.NET-Q8-AnyCPU: 7.22.3 → 14.10.2 (SECURITY FIX)
2. Microsoft.AspNetCore.SpaServices.Extensions: 5.0.1 → 10.0.3
3. Microsoft.EntityFrameworkCore.Design: 5.0.1 → 10.0.3
4. Microsoft.EntityFrameworkCore.Proxies: 5.0.1 → 10.0.3
5. Microsoft.EntityFrameworkCore.Sqlite: 5.0.1 → 10.0.3
6. Microsoft.EntityFrameworkCore.Tools: 5.0.1 → 10.0.3

**Packages Remaining Compatible** (4 total):
1. NSwag.AspNetCore: 13.9.4 (no change needed)
2. SpotifyAPI.Web: 6.0.0 (no change needed)
3. SpotifyAPI.Web.Auth: 6.0.0 (no change needed)
4. TagLibSharp: 2.2.0 (no change needed)

### Step 4: Address Breaking Changes

See §Breaking Changes Catalog for comprehensive details.

**Priority 1: Critical Breaking Changes**

1. **SpaServices.Extensions Deprecation**
   - Issue: Microsoft.AspNetCore.SpaServices.Extensions deprecated in .NET 6+
   - Location: Startup.cs / Program.cs
   - Resolution: Remove SpaServices.Extensions package, use built-in SPA support
   - Files to update: Program.cs, Startup.cs
   - Impact: Requires middleware configuration changes

2. **Magick.NET API Changes (v14)**
   - Issue: MagickGeometry Width/Height changed from int to uint
   - Location: Any code using MagickGeometry properties
   - Resolution: Add explicit (uint) casts where needed
   - Search pattern: `MagickGeometry.*\.(Width|Height)`
   - Example fix: `geometry.Width = (uint)width;`

3. **WebClient Obsolescence**
   - Issue: WebClient obsolete in .NET 6+
   - Location: Any HTTP client code
   - Resolution: Replace with HttpClient via DI
   - Search pattern: `WebClient`
   - Mitigation: Register HttpClient in DI container

**Priority 2: Framework Evolution Changes**

4. **ASP.NET Core Hosting Model**
   - Issue: Startup.cs pattern evolved to minimal hosting in .NET 6+
   - Current: Separate Startup.cs class
   - Target: May keep Startup.cs or migrate to Program.cs minimal API
   - Decision: Keep existing pattern initially, migrate if needed

5. **Entity Framework Core Updates**
   - Issue: EF Core API evolution across versions
   - Resolution: Rely on package upgrade, monitor for warnings
   - Watch for: Deprecated APIs, changed behaviors

### Step 5: Build and Validate

**Build Commands**:
```bash
# Restore packages
dotnet restore Covers.sln

# Build solution
dotnet build Covers.sln --configuration Debug --no-restore

# Build in Release mode
dotnet build Covers.sln --configuration Release --no-restore
```

**Validation Checklist**:
- [ ] No build errors
- [ ] No build warnings
- [ ] All packages restored successfully
- [ ] SPA compiles (npm build in ClientApp)

### Step 6: Runtime Verification

**Manual Testing**:
1. Start application: `dotnet run --project Covers.csproj`
2. Navigate to application URL
3. Verify SPA loads correctly
4. Test core functionality:
   - Album cover display
   - Spotify integration
   - Image processing features
   - Database operations

---

## Package Update Reference

### Complete Package Matrix

| Package Name | Current Version | Target Version | Update Type | Notes |
|:------------|:---------------:|:--------------:|:-----------:|:------|
| Magick.NET-Q8-AnyCPU | 7.22.3 | 14.10.2 | Major | **SECURITY VULNERABILITY** - Breaking changes in API (int → uint) |
| Microsoft.AspNetCore.SpaServices.Extensions | 5.0.1 | 10.0.3 | Major | **DEPRECATED** - Remove and use built-in SPA support |
| Microsoft.EntityFrameworkCore.Design | 5.0.1 | 10.0.3 | Major | Framework alignment |
| Microsoft.EntityFrameworkCore.Proxies | 5.0.1 | 10.0.3 | Major | Framework alignment |
| Microsoft.EntityFrameworkCore.Sqlite | 5.0.1 | 10.0.3 | Major | Framework alignment |
| Microsoft.EntityFrameworkCore.Tools | 5.0.1 | 10.0.3 | Major | Framework alignment |
| NSwag.AspNetCore | 13.9.4 | 13.9.4 | None | ✅ Compatible - no update needed |
| SpotifyAPI.Web | 6.0.0 | 6.0.0 | None | ✅ Compatible - no update needed |
| SpotifyAPI.Web.Auth | 6.0.0 | 6.0.0 | None | ✅ Compatible - no update needed |
| TagLibSharp | 2.2.0 | 2.2.0 | None | ✅ Compatible - no update needed |

### Package Update Categories

**Category 1: Security-Critical** (1 package)
- Magick.NET-Q8-AnyCPU: Must update to resolve vulnerability

**Category 2: Framework Packages** (5 packages)
- All Microsoft.EntityFrameworkCore.* packages
- Microsoft.AspNetCore.SpaServices.Extensions (with deprecation handling)

**Category 3: Compatible** (4 packages)
- No changes required
- Already compatible with .NET 10

---

## Breaking Changes Catalog

### Known Breaking Changes from User Context

#### 1. WebClient Obsolescence
**Introduced**: .NET 6.0  
**Severity**: High  
**Impact**: Compilation error if WebClient is used

**Current Pattern** (if exists):
```csharp
var client = new WebClient();
var data = client.DownloadString(url);
```

**Updated Pattern**:
```csharp
// In Startup.cs ConfigureServices
services.AddHttpClient();

// In consuming class
public class MyService
{
    private readonly IHttpClientFactory _httpClientFactory;
    
    public MyService(IHttpClientFactory httpClientFactory)
    {
        _httpClientFactory = httpClientFactory;
    }
    
    public async Task<string> GetDataAsync(string url)
    {
        var client = _httpClientFactory.CreateClient();
        return await client.GetStringAsync(url);
    }
}
```

**Files Likely Affected**:
- Search for: `WebClient`
- Register: `services.AddHttpClient()` in Startup.cs

#### 2. Magick.NET v14 API Changes
**Introduced**: Magick.NET v14.x  
**Severity**: Medium  
**Impact**: Compilation error where Width/Height properties are used

**Change**: Width and Height properties changed from `int` to `uint`

**Current Pattern**:
```csharp
var geometry = new MagickGeometry();
geometry.Width = width;  // width is int
geometry.Height = height;  // height is int
```

**Updated Pattern**:
```csharp
var geometry = new MagickGeometry();
geometry.Width = (uint)width;  // explicit cast to uint
geometry.Height = (uint)height;  // explicit cast to uint
```

**Files Likely Affected**:
- Search for: `MagickGeometry`
- Likely locations: Image processing services

#### 3. SpaServices.Extensions Deprecation
**Introduced**: .NET 6.0  
**Severity**: High  
**Impact**: Package deprecated, alternative approach needed

**Current Pattern** (from project file):
```xml
<PackageReference Include="Microsoft.AspNetCore.SpaServices.Extensions" Version="5.0.1" />
```

**Current Usage** (likely in Startup.cs):
```csharp
services.AddSpaStaticFiles(configuration =>
{
    configuration.RootPath = "ClientApp/build";
});

app.UseSpaStaticFiles();
app.UseSpa(spa =>
{
    spa.Options.SourcePath = "ClientApp";
    // ...
});
```

**Updated Pattern**:
Remove package dependency and use built-in middleware:
```csharp
// In Program.cs or Startup.cs
app.UseStaticFiles();
app.MapFallbackToFile("index.html");
```

**Alternative**: Continue using the package if it still functions, but monitor for full removal in future versions.

**Files to Update**:
- Covers.csproj (remove or keep package reference)
- Startup.cs or Program.cs (update middleware configuration)

### Potential Breaking Changes (Framework Evolution)

#### 4. ASP.NET Core Configuration
**Versions**: .NET 6-10  
**Impact**: Medium  
**Description**: Configuration and startup patterns evolved

**Watch For**:
- Minimal hosting API (Program.cs vs Startup.cs)
- WebApplicationBuilder usage
- Service registration patterns
- Middleware ordering

**Mitigation**: 
- Keep existing Startup.class pattern if it still works
- Migrate to minimal API only if required or beneficial

#### 5. Entity Framework Core Behaviors
**Versions**: EF Core 5 → 10  
**Impact**: Low to Medium  
**Description**: Query behaviors and tracking changes

**Watch For**:
- Client/server evaluation changes
- Tracking behavior modifications
- Query translation improvements
- Migration generation differences

**Mitigation**:
- Monitor build warnings
- Test database operations thoroughly
- Review EF Core migration history

#### 6. JSON Serialization
**Versions**: .NET 5-10  
**Impact**: Low  
**Description**: System.Text.Json is default (not Newtonsoft.Json)

**Watch For**:
- If code relies on Newtonsoft.Json specific behaviors
- Serialization settings differences
- Custom converters

**Mitigation**:
- ASP.NET Core uses System.Text.Json by default
- Can add Newtonsoft.Json if needed: `.AddNewtonsoftJson()`

---

## Testing Strategy

### Build Validation (Continuous)

After each change category:
- [ ] `dotnet restore` succeeds
- [ ] `dotnet build` succeeds with 0 errors
- [ ] `dotnet build` succeeds with 0 warnings

### Phase-Based Testing

**Phase 1 Testing** (After framework + package updates):
- [ ] Project file syntax valid
- [ ] All packages restore successfully
- [ ] Solution builds (errors expected, OK at this point)

**Phase 2 Testing** (After breaking changes fixed):
- [ ] Solution builds with 0 errors
- [ ] Solution builds with 0 warnings
- [ ] All code compiles cleanly

**Phase 3 Testing** (Functional verification):
- [ ] Application starts successfully
- [ ] SPA loads and renders
- [ ] Database connection works
- [ ] Spotify API integration functional
- [ ] Image processing works (Magick.NET)
- [ ] Album cover retrieval works
- [ ] No runtime exceptions

### Critical Test Paths

1. **Application Startup**
   - Run: `dotnet run --project Covers.csproj`
   - Verify: No exceptions, server starts

2. **SPA Functionality**
   - Navigate to application URL
   - Verify: React app loads and is interactive

3. **Database Operations**
   - Test: Data retrieval from SQLite
   - Verify: EF Core migrations compatible

4. **Image Processing**
   - Test: Cover image processing with Magick.NET
   - Verify: Width/Height casts work correctly

5. **External API**
   - Test: Spotify API authentication and data retrieval
   - Verify: API client still functional

### Regression Prevention

**Build Warnings = Failures**:
- Target: Zero warnings policy
- Rationale: Warnings often indicate future breaking changes
- Action: Fix all warnings before proceeding

**Code Search Validation**:
- Search for deprecated APIs before completion
- Search for obsolete attribute usage
- Verify no compiler warnings ignored

---

## Source Control Strategy

### Approach: Single Atomic Commit

**Rationale**:
- Single project upgrade
- All changes logically related
- Easier to revert if needed
- Clear upgrade boundary

### Commit Strategy

**Commit Message Template**:
```
Upgrade Covers solution to .NET 10.0

- Update target framework: net5.0 → net10.0
- Update Microsoft.EntityFrameworkCore.* packages: 5.0.1 → 10.0.3
- Update Magick.NET-Q8-AnyCPU: 7.22.3 → 14.10.2 (security fix)
- Update Microsoft.AspNetCore.SpaServices.Extensions: 5.0.1 → 10.0.3
- Fix WebClient obsolescence (replaced with HttpClient via DI)
- Fix Magick.NET API changes (int → uint casts)
- Fix SpaServices.Extensions deprecation
- Verify clean build with 0 errors and 0 warnings

Resolves security vulnerability in Magick.NET
```

**Commit Timing**: After Phase 4 (Finalization), when all validation passes

---

## Success Criteria

### Technical Criteria

✅ **The upgrade is complete and successful when:**

1. **Framework**
   - [ ] Covers.csproj targets net10.0
   - [ ] .NET 10 SDK used for build

2. **Packages**
   - [ ] All 6 packages updated to target versions
   - [ ] No package dependency conflicts
   - [ ] No security vulnerabilities remain
   - [ ] 4 compatible packages unchanged

3. **Build Quality**
   - [ ] `dotnet restore` succeeds
   - [ ] `dotnet build` succeeds with 0 errors
   - [ ] `dotnet build` succeeds with 0 warnings
   - [ ] Release configuration builds successfully

4. **Code Quality**
   - [ ] All breaking changes resolved
   - [ ] WebClient replaced with HttpClient (if present)
   - [ ] Magick.NET API changes applied (uint casts)
   - [ ] SpaServices.Extensions deprecation handled
   - [ ] No obsolete API usage warnings

5. **Functionality**
   - [ ] Application starts without errors
   - [ ] SPA builds and loads correctly
   - [ ] Database operations functional
   - [ ] Image processing works (Magick.NET)
   - [ ] Spotify integration operational
   - [ ] Core features verified

6. **Process**
   - [ ] All changes committed on upgrade branch
   - [ ] tasks.md updated with status
   - [ ] Clean working tree

### Acceptance Criteria

**Definition of Done**:
- All technical criteria met
- Zero build errors
- Zero build warnings  
- Application runs successfully
- Core functionality verified
- Changes committed with descriptive message

---

## Rollback Plan

### Rollback Scenarios

**Scenario 1: Upgrade Fails - Cannot Build**
- Action: Revert to previous commit
- Command: `git reset --hard HEAD~1`
- Restore: Source branch unchanged, can restart

**Scenario 2: Builds But Runtime Issues**
- Action: Debug specific issues or revert
- Option 1: Fix issues incrementally
- Option 2: Revert and reassess approach

**Scenario 3: Breaking Changes Too Extensive**
- Action: Consider incremental upgrade path
- Alternative: Upgrade to .NET 6 first, then to .NET 10

### Recovery Steps

1. Ensure source branch intact (do not force push)
2. Document issues encountered before rollback
3. Reset upgrade branch to last known good state
4. Reassess strategy if needed
5. Preserve learning for next attempt

---

## Dependencies and Prerequisites

### Required Software

1. **.NET 10 SDK**
   - Version: 10.0.x or later
   - Download: https://dot.net/download/dotnet/10.0
   - Verification: `dotnet --list-sdks`

2. **Node.js** (for SPA build)
   - Required for ClientApp build
   - Version: Check package.json for requirements

3. **Git**
   - For version control operations
   - On upgrade branch: copilot/upgrade-covers-sln-dotnet-10

### Environment Verification

Before starting:
- [ ] .NET 10 SDK installed and available
- [ ] Node.js installed and in PATH
- [ ] Git repository in clean state
- [ ] On correct branch (copilot/upgrade-covers-sln-dotnet-10)
- [ ] Internet connection available (for package restore)

---

## Assumptions and Constraints

### Assumptions

1. **SDK Availability**: .NET 10 SDK is installed
2. **Package Versions**: Suggested versions in assessment are current and available
3. **Code Patterns**: No extensive use of obsolete APIs beyond those documented
4. **Testing**: Manual testing is sufficient (no automated test suite mentioned)
5. **SPA Build**: ClientApp build process remains compatible

### Constraints

1. **Single Project**: Only Covers.csproj in solution
2. **Branch**: Must work on copilot/upgrade-covers-sln-dotnet-10
3. **Zero Warnings**: Must achieve clean build with no warnings
4. **Known Issues**: Must address user-documented breaking changes
5. **Security**: Must resolve Magick.NET vulnerability

### Areas of Uncertainty

1. **SpaServices.Extensions Replacement**: Exact migration path may require experimentation
2. **Magick.NET API Changes**: May be additional API changes beyond Width/Height
3. **WebClient Usage**: Extent of WebClient usage unknown (may not exist)
4. **Runtime Behaviors**: EF Core and ASP.NET Core behavior changes may emerge during testing

---

## Next Steps

After plan approval, proceed to Execution stage where:

1. **Preparation**: Verify environment and prerequisites
2. **Framework Update**: Update project file to net10.0
3. **Package Update**: Update all 6 packages to target versions
4. **Breaking Changes**: Apply known fixes and resolve compilation errors
5. **Validation**: Build, test, and verify functionality
6. **Finalization**: Commit changes and update task tracking

The plan will be translated into executable tasks with clear success criteria and validation checkpoints.

---

*This plan provides the blueprint for the Execution stage, where actual changes will be applied systematically.*
