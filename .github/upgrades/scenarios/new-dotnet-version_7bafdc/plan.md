# .NET 10 Upgrade Plan for Covers.sln

**Date**: February 17, 2026  
**Target Framework**: .NET 10.0  
**Current Framework**: .NET 5.0  
**Strategy**: All-At-Once

---

## Executive Summary

This plan outlines the upgrade of Covers.sln from .NET 5.0 to .NET 10.0. The solution contains a single ASP.NET Core web application with a React SPA frontend.

### Selected Strategy

**All-At-Once Strategy** - All components upgraded simultaneously in a single coordinated operation.

**Rationale**:
- Single project solution (minimal complexity)
- Currently on .NET 5.0 (modern .NET baseline)
- All packages have .NET 10 compatible versions available
- No API breaking changes detected
- Low difficulty assessment (🟢 Low)
- 3,099 LOC across 51 files

### Key Metrics from Assessment

| Metric | Value |
|--------|-------|
| Total Projects | 1 |
| Total NuGet Packages | 10 |
| Packages Requiring Upgrade | 6 (60%) |
| Compatible Packages | 4 (40%) |
| API Breaking Changes | 0 |
| Difficulty Level | 🟢 Low |

---

## Implementation Timeline

### Phase 0: Preparation

**Operations**:
- Verify .NET 10 SDK is installed
- Ensure working on upgrade branch: `copilot/upgrade-covers-sln-to-dotnet10`

**Deliverables**: Environment ready for upgrade

### Phase 1: Atomic Upgrade

**Operations** (performed as single coordinated batch):
1. Update project file target framework from net5.0 → net10.0
2. Update all NuGet package references to .NET 10 compatible versions
3. Restore dependencies
4. Build solution and identify compilation errors (if any)
5. Fix compilation errors and warnings
6. Build solution with 0 errors and 0 warnings

**Deliverables**: 
- Solution builds successfully
- All warnings resolved
- No compilation errors

### Phase 2: Test Validation

**Operations**:
- Verify application starts successfully
- Test key functionality (if test infrastructure exists)
- Address any runtime issues discovered

**Deliverables**: 
- Application runs successfully
- No runtime regressions detected

### Phase 3: Finalization

**Operations**:
- Final build verification
- Review changes
- Document any behavioral differences

**Deliverables**: Upgrade complete and validated

---

## Detailed Execution Steps

### Step 1: Update Project Target Framework

**File**: `Covers.csproj`

**Change**:
```xml
<TargetFramework>net5.0</TargetFramework>
```

**To**:
```xml
<TargetFramework>net10.0</TargetFramework>
```

### Step 2: Update Package References

Update all packages in `Covers.csproj` to their .NET 10 compatible versions:

#### Critical Security Update
- **Magick.NET-Q8-AnyCPU**: 7.22.3 → 14.10.2
  - **Reason**: Security vulnerability
  - **Priority**: CRITICAL - Must be updated immediately

#### Microsoft Package Updates
- **Microsoft.AspNetCore.SpaServices.Extensions**: 5.0.1 → 10.0.3
- **Microsoft.EntityFrameworkCore.Design**: 5.0.1 → 10.0.3
- **Microsoft.EntityFrameworkCore.Proxies**: 5.0.1 → 10.0.3
- **Microsoft.EntityFrameworkCore.Sqlite**: 5.0.1 → 10.0.3
- **Microsoft.EntityFrameworkCore.Tools**: 5.0.1 → 10.0.3

#### Compatible Packages (No Change Required)
- **NSwag.AspNetCore**: 13.9.4 ✅
- **SpotifyAPI.Web**: 6.0.0 ✅
- **SpotifyAPI.Web.Auth**: 6.0.0 ✅
- **TagLibSharp**: 2.2.0 ✅

### Step 3: Restore and Build

**Commands**:
```bash
dotnet restore Covers.sln
dotnet build Covers.sln --no-restore
```

**Expected Outcome**: Clean build with 0 errors

### Step 4: Address Warnings

**Objective**: Fix all compiler warnings to ensure code quality.

**Approach**:
- Review all warnings from build output
- Address each warning systematically
- Rebuild to verify warnings are resolved
- Repeat until 0 warnings remain

**Common .NET 5 → .NET 10 Warning Categories**:
- Nullable reference type warnings
- Deprecated API usage warnings
- Platform-specific API warnings

### Step 5: Verify Application Functionality

**Testing Approach**:
- Run the application: `dotnet run --project Covers.csproj`
- Verify web server starts successfully
- Test critical application paths
- Verify SPA frontend loads correctly
- Test database connectivity (EF Core)

---

## Package Update Reference

### Complete Package Matrix

| Package | Current | Target | Category | Impact |
|---------|---------|--------|----------|--------|
| Magick.NET-Q8-AnyCPU | 7.22.3 | 14.10.2 | Security | HIGH - Security vulnerability fix |
| Microsoft.AspNetCore.SpaServices.Extensions | 5.0.1 | 10.0.3 | Framework | MEDIUM - SPA integration |
| Microsoft.EntityFrameworkCore.Design | 5.0.1 | 10.0.3 | Framework | MEDIUM - EF Core design-time tools |
| Microsoft.EntityFrameworkCore.Proxies | 5.0.1 | 10.0.3 | Framework | MEDIUM - EF Core lazy loading |
| Microsoft.EntityFrameworkCore.Sqlite | 5.0.1 | 10.0.3 | Framework | MEDIUM - Database provider |
| Microsoft.EntityFrameworkCore.Tools | 5.0.1 | 10.0.3 | Framework | MEDIUM - EF Core CLI tools |
| NSwag.AspNetCore | 13.9.4 | - | Compatible | LOW - No update needed |
| SpotifyAPI.Web | 6.0.0 | - | Compatible | LOW - No update needed |
| SpotifyAPI.Web.Auth | 6.0.0 | - | Compatible | LOW - No update needed |
| TagLibSharp | 2.2.0 | - | Compatible | LOW - No update needed |

### Package Update Notes

#### Magick.NET-Q8-AnyCPU (7.22.3 → 14.10.2)
- **Breaking Change Risk**: MEDIUM
- **Major version jump** (v7 → v14) indicates potential breaking changes
- **Action Required**: Test image processing functionality thoroughly after upgrade
- **API Changes**: Review Magick.NET v14 release notes for breaking changes

#### Microsoft.EntityFrameworkCore.* (5.0.1 → 10.0.3)
- **Breaking Change Risk**: LOW to MEDIUM
- **Provider Changes**: Sqlite provider may have behavioral changes
- **Migration Considerations**: Existing migrations should remain compatible
- **Action Required**: Test database operations and migrations

#### Microsoft.AspNetCore.SpaServices.Extensions (5.0.1 → 10.0.3)
- **Breaking Change Risk**: LOW
- **Notes**: This package facilitates SPA integration with ASP.NET Core
- **Action Required**: Verify SPA startup and proxy configuration

---

## Breaking Changes Catalog

### Framework Breaking Changes (.NET 5 → .NET 10)

Based on assessment: **0 binary incompatible APIs detected**

#### Known .NET 5 → .NET 10 Changes to Monitor

1. **Nullable Reference Types**
   - .NET 6+ has stricter nullable reference type checks
   - May generate new warnings in existing code
   - Action: Enable nullable warnings and address systematically

2. **ASP.NET Core Changes**
   - Minimal APIs patterns (new feature)
   - Updated middleware registration patterns
   - Enhanced configuration binding
   - Action: Review Startup.cs and Program.cs patterns

3. **Entity Framework Core Changes**
   - EF Core 10 includes performance improvements
   - Query behavior changes in some edge cases
   - Action: Review query patterns if unexpected results occur

4. **Performance Improvements**
   - .NET 10 includes significant performance enhancements
   - May change timing-sensitive code behavior
   - Action: Monitor performance-critical paths

### Package-Specific Breaking Changes

#### Magick.NET v7 → v14
- **Risk**: HIGH (major version jump)
- **Potential Issues**:
  - API signature changes
  - Method renames or removals
  - Changed default behaviors
  - New constructor requirements
- **Mitigation**:
  - Review release notes: https://github.com/dlemstra/Magick.NET/releases
  - Test all image processing operations
  - Update code as needed for API changes

#### NSwag.AspNetCore
- **Risk**: LOW (package is compatible)
- **Notes**: Version 13.9.4 supports .NET 10
- **Action**: Verify OpenAPI/Swagger generation works correctly

---

## Risk Assessment

### Overall Risk Level: 🟢 LOW to MEDIUM

The upgrade carries low overall risk due to:
- ✅ Single project solution
- ✅ Modern starting framework (.NET 5.0)
- ✅ No breaking API changes detected
- ✅ Small codebase (3,099 LOC)
- ✅ SDK-style project format
- ⚠️ Magick.NET major version jump requires attention

### Risk Categories

#### High Risk Items

1. **Magick.NET Security Update (7.22.3 → 14.10.2)**
   - **Risk**: Major version jump may introduce breaking changes
   - **Impact**: Image processing functionality may break
   - **Mitigation**: 
     - Test all image processing features thoroughly
     - Review Magick.NET changelog for breaking changes
     - Have rollback plan ready
     - Consider staged testing of image processing

#### Medium Risk Items

1. **Entity Framework Core Update (5.0.1 → 10.0.3)**
   - **Risk**: Query behavior changes
   - **Impact**: Database operations may behave differently
   - **Mitigation**:
     - Test all database operations
     - Verify migrations work correctly
     - Check for lazy loading behavior changes
     - Monitor query performance

2. **ASP.NET Core Update**
   - **Risk**: Middleware or configuration changes
   - **Impact**: Application startup or routing issues
   - **Mitigation**:
     - Test application startup
     - Verify all routes work correctly
     - Check authentication/authorization
     - Test SPA integration

#### Low Risk Items

1. **Compatible Packages**
   - NSwag, SpotifyAPI, TagLibSharp all compatible
   - Minimal testing required

### Risk Mitigation Strategy

**For Each Risk Level**:

**High Risk (Magick.NET)**:
1. Review release notes and changelog
2. Create comprehensive test cases for image operations
3. Test in isolated environment first
4. Document any required code changes
5. Validate with sample images

**Medium Risk (EF Core, ASP.NET Core)**:
1. Run existing tests (if available)
2. Manual testing of key features
3. Monitor logs for warnings
4. Performance regression testing

**Low Risk (Compatible Packages)**:
1. Smoke testing
2. Verify no runtime errors

---

## Testing Strategy

### Multi-Level Testing Approach

#### Build Validation
- **Objective**: Ensure solution compiles successfully
- **Approach**: 
  - `dotnet build Covers.sln`
  - Verify 0 errors, 0 warnings
  - Check for nullable reference type warnings

#### Runtime Validation
- **Objective**: Ensure application starts and runs
- **Approach**:
  - `dotnet run --project Covers.csproj`
  - Verify web server starts on expected port
  - Access application in browser
  - Verify SPA loads correctly

#### Feature Validation

**Critical Paths to Test**:

1. **SPA Frontend**
   - React application loads
   - Client-side routing works
   - API calls to backend succeed

2. **Backend API**
   - All controllers respond correctly
   - API documentation (NSwag/Swagger) generates
   - Authentication/authorization works

3. **Database Operations**
   - EF Core context initializes
   - Database queries execute successfully
   - Migrations (if any) work correctly
   - Lazy loading (Proxies) functions correctly

4. **Image Processing**
   - Magick.NET operations execute
   - Image uploads and processing work
   - Verify quality and output

5. **Third-Party Integrations**
   - Spotify API integration works
   - Tag library operations succeed

#### Testing Checklist

Per-project validation (Covers.csproj):
- [x] Project builds without errors
- [x] Project builds without warnings
- [x] Application starts successfully
- [x] Web server responds to requests
- [x] SPA frontend loads
- [x] Database operations work
- [x] Image processing works
- [x] Third-party API integrations work
- [x] No security vulnerabilities remain

---

## Source Control Strategy

### Recommended Approach: Single Commit

**Rationale**:
- Atomic upgrade operation
- All changes are interdependent
- Easier to review as a complete unit
- Simpler to rollback if needed

### Commit Structure

```
git add .
git commit -m "Upgrade Covers.sln from .NET 5.0 to .NET 10.0

- Update target framework to net10.0
- Update Magick.NET-Q8-AnyCPU 7.22.3 → 14.10.2 (security fix)
- Update Microsoft packages 5.0.1 → 10.0.3
- Fix all compilation warnings
- Verify build and runtime functionality

Resolves: upgrade to .NET 10.0"
```

### Alternative: Staged Commits (if needed)

If issues arise, consider:
1. **Commit 1**: Framework and compatible package updates
2. **Commit 2**: Magick.NET update separately (due to high risk)
3. **Commit 3**: Warning fixes and code adjustments

---

## Success Criteria

The upgrade is considered **complete and successful** when:

### Technical Criteria
1. ✅ Project targets .NET 10.0
2. ✅ All packages updated to specified versions
3. ✅ Solution builds with **0 errors**
4. ✅ Solution builds with **0 warnings**
5. ✅ No package dependency conflicts
6. ✅ No security vulnerabilities remain
7. ✅ Application starts successfully
8. ✅ All critical features function correctly

### Quality Criteria
1. ✅ Code quality maintained or improved
2. ✅ No performance regressions detected
3. ✅ Logging and error handling work correctly
4. ✅ Configuration loads properly

### Documentation Criteria
1. ✅ Changes documented in commit message
2. ✅ Any breaking changes noted
3. ✅ Known issues documented (if any)

---

## Rollback Plan

If critical issues arise during or after upgrade:

### Immediate Rollback
```bash
# Revert to previous state
git reset --hard HEAD~1

# Or checkout previous branch
git checkout <previous-branch>
```

### Partial Rollback

If only specific package causes issues:
1. Revert problematic package to previous version
2. Rebuild and test
3. Investigate issue
4. Attempt upgrade again with fixes

### Recovery Strategy

1. **Identify Issue**: Determine exact failure point
2. **Isolate Cause**: Test packages individually if needed
3. **Research Solution**: Check package release notes, GitHub issues
4. **Apply Fix**: Update code or configuration as needed
5. **Retest**: Verify fix resolves issue
6. **Proceed**: Continue with upgrade

---

## Known Limitations and Considerations

### Magick.NET Major Version Jump
- **Concern**: Version 7 → 14 is a significant jump (7 major versions)
- **Consideration**: Extensive API changes are likely
- **Recommendation**: Budget extra time for image processing testing and potential code updates

### SpaServices.Extensions
- **Note**: This package is in maintenance mode
- **Consideration**: Microsoft recommends alternative approaches for SPA integration in newer projects
- **Action**: Current upgrade is fine, but consider migration path in future

### Entity Framework Core 10
- **Performance**: EF Core 10 includes significant performance improvements
- **Consideration**: Existing queries may execute faster or differently
- **Action**: Monitor query performance and behavior

### .NET 10 End of Life
- **Long-Term Support**: .NET 10 is an LTS release (November 2026 - November 2029)
- **Consideration**: Plan next upgrade before November 2029

---

## Post-Upgrade Recommendations

After successful upgrade:

1. **Monitor Production**:
   - Watch for any unexpected behaviors
   - Monitor performance metrics
   - Check error logs for new issues

2. **Update Documentation**:
   - Update README with .NET 10 requirement
   - Document any code changes made
   - Update deployment documentation

3. **Developer Communication**:
   - Notify team of framework upgrade
   - Share any new patterns or best practices
   - Update development environment setup docs

4. **Continuous Improvement**:
   - Review warnings that were addressed
   - Consider adopting new .NET 10 features
   - Plan for future dependency updates

---

## Appendix: Version History

| Date | Framework | Notes |
|------|-----------|-------|
| Original | .NET 5.0 | Initial framework version |
| Feb 2026 | .NET 10.0 | Upgrade to LTS release |

---

## Appendix: Useful Resources

**Official Documentation**:
- [.NET 10 Release Notes](https://github.com/dotnet/core/tree/main/release-notes/10.0)
- [Breaking Changes in .NET 10](https://learn.microsoft.com/en-us/dotnet/core/compatibility/10.0)
- [Migrating from .NET 5 to .NET 10](https://learn.microsoft.com/en-us/dotnet/core/migration/)

**Package Documentation**:
- [Magick.NET Documentation](https://github.com/dlemstra/Magick.NET)
- [EF Core 10 What's New](https://learn.microsoft.com/en-us/ef/core/what-is-new/ef-core-10.0/whatsnew)
- [ASP.NET Core 10 What's New](https://learn.microsoft.com/en-us/aspnet/core/release-notes/aspnetcore-10.0)

**Tools**:
- [.NET Upgrade Assistant](https://dotnet.microsoft.com/platform/upgrade-assistant)
- [API Port](https://github.com/microsoft/dotnet-apiport)

---

*This plan provides a comprehensive blueprint for upgrading Covers.sln to .NET 10.0. It will be executed in the Execution stage, where each step will be performed systematically and validated.*
