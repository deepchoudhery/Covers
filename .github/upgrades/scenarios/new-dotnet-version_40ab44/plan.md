# .NET 10.0 Upgrade Plan

**Date**: 2026-02-19  
**Upgrade Strategy**: All-At-Once  
**Source Framework**: .NET 5.0  
**Target Framework**: .NET 10.0

---

## Executive Summary

This plan outlines the upgrade of the Covers solution from .NET 5.0 to .NET 10.0. The solution consists of a single ASP.NET Core web application with React frontend. The assessment indicates this is a **low-difficulty upgrade** with no API compatibility issues detected and minimal code impact expected.

**Key Statistics:**
- **Projects**: 1 (Covers.csproj)
- **Total Code**: 3,099 lines across 51 files
- **Package Updates**: 6 of 10 packages require updates
- **Security Issues**: 1 package with security vulnerability (Magick.NET-Q8-AnyCPU)
- **API Issues**: 0 breaking changes detected
- **Estimated Code Impact**: 0+ LOC (0.0% of codebase)

**Why All-At-Once Strategy:**
- Single project solution (no complex dependencies)
- Currently on .NET 5.0 (SDK-style project)
- All packages have clear upgrade paths
- No breaking API changes detected
- Low complexity makes simultaneous upgrade optimal

---

## Upgrade Strategy: All-At-Once

### Rationale

The All-At-Once strategy is optimal for this solution because:

1. **Single Project**: Only one project to upgrade eliminates dependency coordination complexity
2. **SDK-Style Project**: Already using modern project format, no conversion needed
3. **Clear Package Path**: All 6 packages requiring updates have identified target versions
4. **No Breaking Changes**: Assessment found zero API compatibility issues
5. **Fast Completion**: Single atomic operation minimizes upgrade duration
6. **Clean Testing**: One comprehensive test cycle covers entire solution

### Approach

All changes will be applied simultaneously:
- Update target framework to net10.0
- Update all 6 packages to their .NET 10-compatible versions
- Validate build succeeds without warnings
- Run comprehensive testing

---

## Dependency Analysis

### Project Structure

```
Covers.csproj (ASP.NET Core Web + React SPA)
└── (No project dependencies - standalone application)
```

**Dependency Characteristics:**
- **Type**: ASP.NET Core Web Application with SPA
- **Current TFM**: net5.0
- **Target TFM**: net10.0
- **Dependencies**: 10 NuGet packages, no project references
- **Complexity**: Low (single project, clear dependency tree)

### Upgrade Order

Since there is only one project, upgrade order is straightforward:
1. Update Covers.csproj target framework
2. Update all package references simultaneously
3. Address any resulting compilation issues
4. Validate and test

---

## Package Update Strategy

### Security-Critical Updates

**Immediate Action Required:**

| Package | Current | Target | Severity | Reason |
|---------|---------|--------|----------|--------|
| Magick.NET-Q8-AnyCPU | 7.22.3 | 14.10.2 | 🔴 High | Contains security vulnerability |

**Action**: This package must be updated as part of the upgrade to eliminate known security risks.

### Framework-Aligned Updates

**Required for .NET 10.0 Compatibility:**

| Package | Current | Target | Category |
|---------|---------|--------|----------|
| Microsoft.AspNetCore.SpaServices.Extensions | 5.0.1 | 10.0.3 | ASP.NET Core |
| Microsoft.EntityFrameworkCore.Design | 5.0.1 | 10.0.3 | Entity Framework Core |
| Microsoft.EntityFrameworkCore.Proxies | 5.0.1 | 10.0.3 | Entity Framework Core |
| Microsoft.EntityFrameworkCore.Sqlite | 5.0.1 | 10.0.3 | Entity Framework Core |
| Microsoft.EntityFrameworkCore.Tools | 5.0.1 | 10.0.3 | Entity Framework Core |

**Rationale**: These Microsoft packages are framework-aligned and must match the target framework version for optimal compatibility and support.

### Already Compatible Packages

**No Update Needed:**

| Package | Current | Status |
|---------|---------|--------|
| NSwag.AspNetCore | 13.9.4 | ✅ Compatible |
| SpotifyAPI.Web | 6.0.0 | ✅ Compatible |
| SpotifyAPI.Web.Auth | 6.0.0 | ✅ Compatible |
| TagLibSharp | 2.2.0 | ✅ Compatible |

**Note**: These packages are already compatible with .NET 10.0 and do not require updates.

### Breaking Changes Assessment

#### Magick.NET v7.22.3 → v14.10.2

**Major Version Jump**: This is a significant upgrade (7.x → 14.x) that may include breaking changes.

**Expected Changes:**
- API signature changes (property types, method signatures)
- Potential namespace reorganization
- New initialization patterns
- Performance improvements and new features

**Mitigation Strategy:**
1. Review Magick.NET v14 release notes and migration guides
2. Identify all usages in the codebase (Controllers, Services, Background Services)
3. Update code for any breaking API changes
4. Test image processing functionality thoroughly

**Files to Review:**
- Controllers/CoverController.cs (likely contains image manipulation code)
- Any services using image processing

#### Microsoft Packages v5.0.1 → v10.0.3

**Framework-Aligned Updates**: Generally well-documented with clear migration paths.

**Expected Changes:**
- ASP.NET Core: Minimal breaking changes (mostly configuration patterns)
- Entity Framework Core: Database migration compatibility, LINQ provider updates
- SpaServices: Potential middleware registration changes

**Mitigation Strategy:**
1. Review .NET 10 breaking changes documentation
2. Check Program.cs and Startup.cs for deprecated patterns
3. Verify middleware pipeline configuration
4. Test EF Core migrations and database operations

---

## Project-Specific Upgrade Plan

### Covers.csproj

**Project Type**: ASP.NET Core Web Application with React SPA  
**Current Framework**: net5.0  
**Target Framework**: net10.0  
**Complexity**: 🟢 Low  
**LOC**: 3,099 lines across 51 files

#### Step 1: Update Project File

**Action**: Update Covers.csproj

```xml
<!-- Change from: -->
<TargetFramework>net5.0</TargetFramework>

<!-- Change to: -->
<TargetFramework>net10.0</TargetFramework>
```

**Package Reference Updates:**

```xml
<!-- Update these packages: -->
<PackageReference Include="Magick.NET-Q8-AnyCPU" Version="14.10.2" />
<PackageReference Include="Microsoft.AspNetCore.SpaServices.Extensions" Version="10.0.3" />
<PackageReference Include="Microsoft.EntityFrameworkCore.Design" Version="10.0.3">
  <PrivateAssets>all</PrivateAssets>
  <IncludeAssets>runtime; build; native; contentfiles; analyzers; buildtransitive</IncludeAssets>
</PackageReference>
<PackageReference Include="Microsoft.EntityFrameworkCore.Proxies" Version="10.0.3" />
<PackageReference Include="Microsoft.EntityFrameworkCore.Sqlite" Version="10.0.3" />
<PackageReference Include="Microsoft.EntityFrameworkCore.Tools" Version="10.0.3">
  <PrivateAssets>all</PrivateAssets>
  <IncludeAssets>runtime; build; native; contentfiles; analyzers; buildtransitive</IncludeAssets>
</PackageReference>

<!-- These remain unchanged (already compatible): -->
<PackageReference Include="NSwag.AspNetCore" Version="13.9.4" />
<PackageReference Include="SpotifyAPI.Web" Version="6.0.0" />
<PackageReference Include="SpotifyAPI.Web.Auth" Version="6.0.0" />
<PackageReference Include="TagLibSharp" Version="2.2.0" />
```

#### Step 2: Restore Packages

**Action**: Run package restore to download .NET 10 compatible packages

```bash
dotnet restore
```

**Expected Outcome**: All packages download successfully without conflicts

#### Step 3: Build and Address Compilation Issues

**Action**: Attempt initial build to identify breaking changes

```bash
dotnet build
```

**Expected Issues:**

1. **Magick.NET Breaking Changes** (if any):
   - Type changes (e.g., `int` → `uint` for size properties)
   - Method signature updates
   - Namespace reorganization

2. **ASP.NET Core Updates**:
   - Deprecated middleware patterns
   - Configuration API changes
   - Authentication/Authorization updates

3. **Entity Framework Core**:
   - LINQ query translation changes
   - Migration compatibility issues

**Resolution Strategy:**
- Address each compilation error systematically
- Consult .NET 10 breaking changes documentation
- Review package-specific migration guides
- Update code to use new APIs where required

#### Step 4: Code Changes

**Primary Focus Areas:**

1. **Program.cs / Startup.cs**
   - Verify middleware registration order
   - Update service configuration for .NET 10 patterns
   - Check authentication/authorization setup
   - Validate SPA configuration

2. **Controllers** (especially CoverController.cs)
   - Update Magick.NET API calls if needed
   - Verify attribute routing compatibility
   - Check action method patterns

3. **Services** (especially image processing)
   - Update Magick.NET usage
   - Verify dependency injection patterns
   - Check async/await patterns

4. **Background Services**
   - Verify hosted service registration
   - Check background task patterns
   - Update any obsolete APIs

5. **Entity Framework Models/Context**
   - Verify DbContext configuration
   - Check model configurations
   - Review LINQ queries for breaking changes

#### Step 5: Validate No Warnings

**Action**: Ensure clean build with zero warnings

```bash
dotnet build --no-restore
```

**Acceptance Criteria:**
- Build succeeded: Yes
- Errors: 0
- Warnings: 0

**If Warnings Exist:**
- Review each warning
- Update code to eliminate warnings
- Do not suppress warnings without understanding root cause

#### Step 6: Test Database Migrations

**Action**: Verify Entity Framework migrations work correctly

```bash
# Check for migration issues
dotnet ef migrations list
```

**Considerations:**
- SQLite database compatibility (should be fine)
- Migration history integrity
- Model changes (should be none unless EF Core has breaking changes)

**If Issues Found:**
- Review EF Core 10.0 breaking changes
- Regenerate migrations if needed
- Test against development database

#### Step 7: Run Tests

**Action**: Execute all tests to verify functionality

```bash
dotnet test
```

**Test Categories:**
1. Unit tests (if present)
2. Integration tests (if present)
3. Manual functional testing:
   - Application startup
   - API endpoints
   - Image processing (cover generation)
   - Database operations
   - SPA frontend functionality

#### Step 8: Runtime Validation

**Action**: Run the application and validate key scenarios

**Validation Checklist:**
- [ ] Application starts without errors
- [ ] Database initializes correctly
- [ ] React SPA loads properly
- [ ] API endpoints respond correctly
- [ ] Image processing works (covers generation)
- [ ] Spotify integration functional
- [ ] Background services run correctly
- [ ] No runtime errors in logs

---

## Breaking Changes Catalog

### Known .NET 5.0 → .NET 10.0 Breaking Changes

**Source**: [Microsoft .NET Breaking Changes Documentation](https://learn.microsoft.com/en-us/dotnet/core/compatibility/)

#### ASP.NET Core

1. **Minimal API Changes**: New hosting model available (but existing Startup.cs pattern still supported)
2. **Authentication**: Some authentication middleware registration patterns updated
3. **CORS**: CORS policy configuration may have subtle changes
4. **Static Files**: Static file middleware configuration updates

**Impact**: Low - Most changes are additive or backwards compatible

#### Entity Framework Core

1. **LINQ Translation**: More strict query translation in EF Core 10
2. **SQLite Provider**: Minor updates to SQLite provider
3. **Migrations**: Migration generation may produce slightly different output

**Impact**: Low - Existing databases and migrations should work

#### Base Class Library

1. **DateTime**: Some date/time parsing behaviors changed
2. **Regex**: Performance improvements with potential behavior changes
3. **HttpClient**: Minor API additions (backwards compatible)

**Impact**: Minimal - Most changes are performance improvements

### Package-Specific Breaking Changes

#### Magick.NET v7.22.3 → v14.10.2

**Confirmed Breaking Changes:**

Based on repository memories and previous upgrade experiences:

1. **MagickGeometry Property Types**:
   - `Width` and `Height` properties changed from `int` to `uint`
   - **Fix**: Cast `int` values to `uint` when creating MagickGeometry
   - **Example**: `new MagickGeometry((uint)width, (uint)height)`

2. **API Surface Changes**:
   - Some method signatures updated
   - Namespace reorganization possible
   - New initialization patterns

**Files Likely Affected:**
- Controllers/CoverController.cs
- Services/SpotifyService.cs (if it processes images)
- BackgroundServices/AlbumScanner.cs (if it processes images)

**Mitigation**:
1. Search codebase for `MagickGeometry` usage
2. Update property access to use `uint` casts
3. Review Magick.NET v14 release notes
4. Test all image processing functionality

---

## Testing Strategy

### Multi-Level Testing

#### Build-Time Validation

**Level 1: Compilation**
- [ ] Project file updates applied correctly
- [ ] All packages restore successfully
- [ ] Solution builds without errors
- [ ] Solution builds without warnings
- [ ] No package dependency conflicts

**Success Criteria**: Clean build with 0 errors, 0 warnings

#### Unit Testing

**Level 2: Automated Tests**
- [ ] All existing unit tests pass
- [ ] No test failures introduced by upgrade
- [ ] Test execution time remains acceptable

**Success Criteria**: 100% test pass rate

#### Integration Testing

**Level 3: Component Integration**
- [ ] Database connectivity and operations
- [ ] API endpoint responses
- [ ] Image processing functionality
- [ ] Spotify API integration
- [ ] Background service execution

**Success Criteria**: All integration points function correctly

#### Functional Testing

**Level 4: End-to-End Scenarios**

**Manual Test Scenarios:**

1. **Application Startup**
   - [ ] Application starts without errors
   - [ ] All services initialize correctly
   - [ ] Database connection established
   - [ ] Configuration loaded properly

2. **Core Functionality**
   - [ ] Create/retrieve album covers
   - [ ] Image processing and resizing
   - [ ] Search functionality works
   - [ ] API returns expected data
   - [ ] Frontend SPA loads and renders

3. **Spotify Integration**
   - [ ] Authentication with Spotify works
   - [ ] Album data retrieval functions
   - [ ] Cover art download successful
   - [ ] Background scanning operates

4. **Data Persistence**
   - [ ] Database CRUD operations work
   - [ ] Migrations apply correctly
   - [ ] Data integrity maintained
   - [ ] SQLite database functions properly

5. **Performance**
   - [ ] Application startup time acceptable
   - [ ] Image processing performance good
   - [ ] API response times normal
   - [ ] Memory usage stable

**Success Criteria**: All scenarios complete successfully without errors

---

## Risk Assessment and Mitigation

### Risk Matrix

| Risk | Likelihood | Impact | Severity | Mitigation |
|------|-----------|--------|----------|------------|
| Magick.NET breaking changes | High | Medium | 🟡 Medium | Review v14 docs, test image processing thoroughly |
| EF Core migration issues | Low | Medium | 🟢 Low | Test migrations, keep backup of development database |
| SpaServices changes | Low | Low | 🟢 Low | Verify SPA middleware configuration |
| Runtime behavior changes | Low | Medium | 🟢 Low | Comprehensive functional testing |
| Package dependency conflicts | Low | Low | 🟢 Low | Clean restore and careful version selection |

### Specific Risk Mitigation

#### High Risk: Magick.NET API Changes

**Mitigation Strategy:**
1. **Pre-Upgrade Research**
   - Review Magick.NET v14.x release notes and changelog
   - Identify all breaking changes between v7 and v14
   - Create list of affected code locations

2. **Systematic Updates**
   - Search for all `MagickGeometry` instantiations
   - Update to use `uint` casts where needed
   - Review other Magick.NET API calls for changes

3. **Comprehensive Testing**
   - Test cover image generation
   - Test image resizing functionality
   - Verify image quality and format
   - Test edge cases (large images, various formats)

4. **Rollback Plan**
   - Keep current version in separate branch
   - Document all changes made
   - Prepare quick rollback procedure

#### Medium Risk: Entity Framework Core Updates

**Mitigation Strategy:**
1. **Database Backup**: Create backup of development database
2. **Migration Validation**: Test migrations in isolated environment
3. **Query Review**: Check LINQ queries for compatibility
4. **Incremental Testing**: Test database operations incrementally

#### Low Risk: General .NET 10 Changes

**Mitigation Strategy:**
1. **Documentation Review**: Check .NET 10 breaking changes
2. **Pattern Validation**: Verify common patterns still work
3. **Configuration Check**: Review appsettings.json and startup code

### Rollback Plan

**If Critical Issues Arise:**

1. **Immediate Rollback**:
   ```bash
   git checkout <previous-commit>
   ```

2. **Restore Packages**:
   ```bash
   dotnet restore
   dotnet build
   ```

3. **Verify Functionality**:
   - Run application
   - Confirm all features work
   - Check no data loss occurred

4. **Document Issues**:
   - Record what failed
   - Capture error messages
   - Identify root cause
   - Plan remediation

**Success Criteria for Rollback**: Application returns to previous working state within 15 minutes

---

## Timeline and Effort Estimation

### Estimated Duration

| Phase | Duration | Effort | Notes |
|-------|----------|--------|-------|
| Project file updates | 15 min | Low | Update TFM and package versions |
| Package restore | 5 min | Automated | Download .NET 10 packages |
| Initial build | 5 min | Automated | Identify compilation issues |
| Fix Magick.NET issues | 1-2 hours | Medium | Update API calls, test image processing |
| Fix other breaking changes | 30 min | Low | Address any .NET 10 changes |
| Testing and validation | 2-3 hours | Medium | Comprehensive functional testing |
| Documentation | 30 min | Low | Update README if needed |
| **Total** | **4-6 hours** | **Medium** | Single developer, full focus |

### Assumptions

- Developer familiar with .NET and the codebase
- Development environment properly configured
- Test data and scenarios available
- No unexpected breaking changes beyond known issues

### Contingency Buffer

- **Add 50% buffer**: 2-3 hours for unexpected issues
- **Total with buffer**: 6-9 hours

---

## Success Criteria

### Technical Criteria

The upgrade is considered **successfully complete** when:

1. **Build Success**
   - [ ] Target framework is net10.0
   - [ ] All 6 packages updated to specified versions
   - [ ] `dotnet build` succeeds with 0 errors
   - [ ] `dotnet build` produces 0 warnings
   - [ ] No package dependency conflicts

2. **Security**
   - [ ] Magick.NET security vulnerability eliminated (v14.10.2 installed)
   - [ ] No new security warnings introduced
   - [ ] All packages up-to-date with .NET 10 compatible versions

3. **Testing**
   - [ ] All automated tests pass
   - [ ] Application starts successfully
   - [ ] Core functionality validated
   - [ ] Image processing works correctly
   - [ ] Database operations function properly
   - [ ] Spotify integration operational
   - [ ] No runtime errors in logs

4. **Code Quality**
   - [ ] No compilation warnings
   - [ ] No runtime warnings
   - [ ] Code follows .NET 10 best practices
   - [ ] No obsolete API usage warnings

### Functional Criteria

- [ ] Users can access the application
- [ ] Album covers can be generated
- [ ] Image processing produces correct results
- [ ] Spotify integration retrieves data
- [ ] Background services run correctly
- [ ] Database operations succeed
- [ ] Frontend SPA loads and functions

### Performance Criteria

- [ ] Application startup time ≤ baseline (or better)
- [ ] Image processing performance ≥ baseline (or better)
- [ ] API response times ≤ baseline (or better)
- [ ] Memory usage ≤ baseline (or better)

### Documentation Criteria

- [ ] README.md updated with .NET 10 requirement (if needed)
- [ ] Any environment setup changes documented
- [ ] Breaking changes documented for team

---

## Post-Upgrade Activities

### Immediate Actions

1. **Commit Changes**
   ```bash
   git add .
   git commit -m "Upgrade to .NET 10.0"
   git push
   ```

2. **Update Documentation**
   - Update README.md with .NET 10 requirement
   - Document any configuration changes
   - Update developer setup instructions

3. **Team Communication**
   - Notify team of upgrade completion
   - Share any breaking changes or new patterns
   - Provide migration notes for local development environments

### Follow-Up Tasks

1. **Monitor Application**
   - Watch for runtime issues in production
   - Monitor performance metrics
   - Check error logs for anomalies

2. **Developer Environment Updates**
   - Ensure all team members have .NET 10 SDK installed
   - Update CI/CD pipelines to use .NET 10
   - Update Docker files if applicable

3. **Future Improvements**
   - Consider adopting .NET 10 new features
   - Review minimal APIs vs traditional controllers
   - Explore performance improvements

---

## Appendix

### Package Update Reference Table

| Package | Current | Target | Change Type | Security | Testing Priority |
|---------|---------|--------|-------------|----------|------------------|
| Magick.NET-Q8-AnyCPU | 7.22.3 | 14.10.2 | Major upgrade | 🔴 Fixes vulnerability | High |
| Microsoft.AspNetCore.SpaServices.Extensions | 5.0.1 | 10.0.3 | Framework-aligned | ✅ None | Medium |
| Microsoft.EntityFrameworkCore.Design | 5.0.1 | 10.0.3 | Framework-aligned | ✅ None | Medium |
| Microsoft.EntityFrameworkCore.Proxies | 5.0.1 | 10.0.3 | Framework-aligned | ✅ None | Medium |
| Microsoft.EntityFrameworkCore.Sqlite | 5.0.1 | 10.0.3 | Framework-aligned | ✅ None | High |
| Microsoft.EntityFrameworkCore.Tools | 5.0.1 | 10.0.3 | Framework-aligned | ✅ None | Low |
| NSwag.AspNetCore | 13.9.4 | (no change) | Compatible | ✅ None | Low |
| SpotifyAPI.Web | 6.0.0 | (no change) | Compatible | ✅ None | High |
| SpotifyAPI.Web.Auth | 6.0.0 | (no change) | Compatible | ✅ None | High |
| TagLibSharp | 2.2.0 | (no change) | Compatible | ✅ None | Low |

### Key Files Reference

**Configuration Files:**
- `Covers.csproj` - Project file with framework and package references
- `appsettings.json` - Application configuration
- `appsettings.Development.json` - Development configuration

**Application Entry Points:**
- `Program.cs` - Application entry point
- `Startup.cs` - Service and middleware configuration

**Core Components:**
- `Controllers/` - API controllers (especially CoverController.cs)
- `Services/` - Business logic services
- `BackgroundServices/` - Background task processing
- `Models/` - Data models
- `Persistency/` - Database context and repositories
- `ClientApp/` - React SPA frontend

### Resources

**Official Documentation:**
- [.NET 10 Release Notes](https://learn.microsoft.com/en-us/dotnet/core/whats-new/dotnet-10)
- [.NET 10 Breaking Changes](https://learn.microsoft.com/en-us/dotnet/core/compatibility/10.0)
- [ASP.NET Core 10 Updates](https://learn.microsoft.com/en-us/aspnet/core/release-notes/aspnetcore-10)
- [Entity Framework Core 10](https://learn.microsoft.com/en-us/ef/core/what-is-new/ef-core-10.0/whatsnew)

**Package Documentation:**
- [Magick.NET Documentation](https://github.com/dlemstra/Magick.NET)
- [Magick.NET v14 Release Notes](https://github.com/dlemstra/Magick.NET/releases)

---

**This plan is ready for Execution stage, where actual changes will be applied systematically.**

*Plan prepared by: Modernization Agent*  
*Assessment basis: /home/runner/work/Covers/Covers/.github/upgrades/scenarios/new-dotnet-version_40ab44/assessment.md*
