# .NET 10 Upgrade Plan for Covers Solution

**Date**: 2026-02-18  
**Target Framework**: .NET 10.0 (Long Term Support)  
**Current Framework**: .NET 5.0  
**Upgrade Strategy**: All-At-Once

---

## Executive Summary

This plan outlines the upgrade of the Covers solution from .NET 5.0 to .NET 10.0. The solution consists of a single ASP.NET Core web application with React frontend, making it an ideal candidate for the all-at-once upgrade strategy.

**Key Highlights:**
- **Difficulty**: 🟢 Low - Single project, no API compatibility issues detected
- **Timeline Estimate**: 2-4 hours total (including testing)
- **Risk Level**: Low - Modern codebase with clear upgrade path
- **Critical Items**: 1 security vulnerability in Magick.NET package must be addressed

---

## Upgrade Strategy

### Strategy Selection: All-At-Once

**Rationale:**
The all-at-once strategy is optimal for this solution because:

1. **Single Project Solution**: Only one project (Covers.csproj) needs upgrading
2. **Modern Foundation**: Already on SDK-style .NET Core project
3. **Clean Dependencies**: No complex dependency graph or project relationships
4. **Clear Package Upgrades**: All 6 packages requiring updates have known compatible versions
5. **No Breaking Changes**: Assessment found 0 API compatibility issues

**Approach:**
- Update target framework from net5.0 to net10.0
- Upgrade all 6 packages simultaneously
- Address Magick.NET security vulnerability
- Build and test in single validation pass

---

## Dependency Analysis

### Project Structure

The solution contains a single project with no project-to-project dependencies:

```
Covers.sln
└── Covers.csproj (ASP.NET Core Web App with React)
    - Current: net5.0
    - Target: net10.0
    - Type: SDK-style
    - LOC: 3,099 across 51 files
```

### Upgrade Order

**Phase 1 (Only Phase):**
- Covers.csproj - Update target framework and all packages

No dependency ordering required since there is only one project.

---

## Package Update Reference

### Packages Requiring Updates

| Package | Current | Target | Priority | Notes |
|---------|---------|--------|----------|-------|
| **Magick.NET-Q8-AnyCPU** | 7.22.3 | 14.10.2 | 🔴 **CRITICAL** | **Security vulnerability** - Major version jump (7→14) |
| Microsoft.AspNetCore.SpaServices.Extensions | 5.0.1 | 10.0.3 | High | Framework alignment |
| Microsoft.EntityFrameworkCore.Design | 5.0.1 | 10.0.3 | High | Framework alignment |
| Microsoft.EntityFrameworkCore.Proxies | 5.0.1 | 10.0.3 | High | Framework alignment |
| Microsoft.EntityFrameworkCore.Sqlite | 5.0.1 | 10.0.3 | High | Framework alignment |
| Microsoft.EntityFrameworkCore.Tools | 5.0.1 | 10.0.3 | High | Framework alignment |

### Packages Already Compatible

| Package | Version | Status |
|---------|---------|--------|
| NSwag.AspNetCore | 13.9.4 | ✅ Compatible |
| SpotifyAPI.Web | 6.0.0 | ✅ Compatible |
| SpotifyAPI.Web.Auth | 6.0.0 | ✅ Compatible |
| TagLibSharp | 2.2.0 | ✅ Compatible |

---

## Breaking Changes Catalog

### Expected Breaking Changes

Based on the upgrade from .NET 5.0 to .NET 10.0:

#### 1. Magick.NET Major Version Update (7.22.3 → 14.10.2)

**Likelihood**: High  
**Impact**: Medium

**Known Changes:**
- API surface changes across major versions 8-14
- Potential property type changes (e.g., int to uint for geometry properties)
- Method signature updates

**Mitigation:**
- Review Magick.NET usage in code (primarily in Controllers/CoverController.cs)
- Consult Magick.NET changelog for breaking changes
- Test image processing functionality thoroughly

#### 2. ASP.NET Core SpaServices.Extensions

**Likelihood**: Low  
**Impact**: Low

**Notes:**
- Package updates from 5.0.1 to 10.0.3
- API is relatively stable for SPA proxy functionality
- Configuration in Startup.cs may need minor adjustments

#### 3. Entity Framework Core (5.0.1 → 10.0.3)

**Likelihood**: Low  
**Impact**: Low

**Potential Changes:**
- LINQ query translation improvements may change query behavior
- SQLite provider updates may affect database operations
- DbContext configuration patterns remain compatible

**Mitigation:**
- Test all database operations after upgrade
- Review EF Core 6, 7, 8, 9, and 10 release notes for cumulative changes
- Ensure migrations still work correctly

### Framework-Level Changes (.NET 5 → 10)

**Areas to Watch:**
1. **HTTP Stack**: Improvements in HTTP/2, HTTP/3 support
2. **JSON Serialization**: System.Text.Json enhancements and defaults
3. **Minimal APIs**: New hosting patterns (project uses traditional Startup pattern)
4. **Performance**: JIT improvements may change hot path behavior

---

## Project-by-Project Plan

### Covers.csproj

**Current State:**
- Target Framework: net5.0
- Type: ASP.NET Core Web App with React SPA
- Lines of Code: 3,099
- Files: 51
- Dependencies: 10 NuGet packages

**Upgrade Steps:**

1. **Update Target Framework**
   - Change `<TargetFramework>net5.0</TargetFramework>` to `<TargetFramework>net10.0</TargetFramework>`

2. **Update Package References** (in order of priority)
   
   a. **Critical Security Fix:**
   ```xml
   <PackageReference Include="Magick.NET-Q8-AnyCPU" Version="14.10.2" />
   ```
   
   b. **Microsoft Packages:**
   ```xml
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
   ```

3. **Restore Packages**
   ```bash
   dotnet restore
   ```

4. **Build Project**
   ```bash
   dotnet build --no-restore
   ```

5. **Fix Compilation Errors** (if any)
   - Address Magick.NET API changes
   - Fix any EF Core query patterns
   - Update any deprecated API usage

6. **Verify Build Success**
   - Build must complete with 0 errors
   - Target: 0 warnings (acceptable threshold: <5 if unavoidable)

**Estimated Effort**: 1-2 hours  
**Risk Level**: 🟢 Low

---

## Testing Strategy

### Multi-Level Testing Approach

#### Level 1: Build Validation
**Objective**: Ensure code compiles successfully

**Steps:**
- [ ] Restore packages successfully
- [ ] Build without errors
- [ ] Build without warnings (or minimal warnings documented)
- [ ] No package dependency conflicts

**Success Criteria**: Clean build with dotnet build

#### Level 2: Unit Testing
**Objective**: Verify core functionality

**Steps:**
- [ ] Run existing unit tests (if any)
- [ ] All tests pass
- [ ] No test compilation errors

**Command**: 
```bash
dotnet test
```

**Success Criteria**: All existing tests pass

#### Level 3: Functional Testing
**Objective**: Validate application behavior

**Critical Paths to Test:**
1. **Application Startup**
   - Application starts without errors
   - React SPA loads correctly
   - SignalR hubs connect properly

2. **Album Scanning**
   - Background service starts
   - Spotify API integration works
   - Album metadata retrieval functions

3. **Cover Art Processing**
   - Image download from Spotify
   - Magick.NET image processing
   - Cover art generation and caching

4. **Database Operations**
   - Entity Framework migrations work
   - SQLite database operations succeed
   - Proxy lazy loading functions correctly

5. **API Endpoints**
   - Swagger/NSwag documentation generation
   - REST API endpoints respond correctly
   - Data serialization works

**Success Criteria**: 
- Application runs without runtime errors
- All critical user flows complete successfully
- Image processing produces correct results

#### Level 4: Performance Validation
**Objective**: Ensure no performance regressions

**Metrics to Compare:**
- Application startup time
- Album scan performance
- Image processing speed
- API response times

**Success Criteria**: Performance within 10% of .NET 5.0 baseline (or better)

---

## Risk Management

### Identified Risks

#### Risk 1: Magick.NET Breaking Changes
**Likelihood**: High  
**Impact**: Medium  
**Severity**: Medium

**Description**: Major version upgrade (7 → 14) likely includes breaking changes

**Mitigation:**
- Review Magick.NET usage before upgrade (Controllers/CoverController.cs, Services/SpotifyService.cs, BackgroundServices/AlbumScanner.cs)
- Check Magick.NET 14.x migration guide
- Test image processing thoroughly
- Have rollback plan ready

**Contingency:**
- If blocking issues found, investigate intermediate versions
- Consider temporary workarounds for deprecated APIs
- Allocate extra time for image processing testing

#### Risk 2: Entity Framework Query Changes
**Likelihood**: Low  
**Impact**: Low  
**Severity**: Low

**Description**: EF Core query translation may change behavior

**Mitigation:**
- Test all database queries after upgrade
- Review EF Core 6-10 breaking changes
- Monitor query performance

**Contingency:**
- Adjust LINQ queries if translation issues occur
- Use explicit FromSqlRaw for problematic queries

#### Risk 3: SPA Integration Issues
**Likelihood**: Low  
**Impact**: Medium  
**Severity**: Low

**Description**: React SPA proxy configuration may need updates

**Mitigation:**
- Test SPA development workflow
- Verify production build process
- Check SpaServices.Extensions release notes

**Contingency:**
- Adjust Startup.cs SPA configuration if needed
- Update npm scripts if required

### Rollback Plan

**If Critical Issues Arise:**

1. **Immediate Rollback:**
   ```bash
   git checkout Covers.csproj
   dotnet restore
   dotnet build
   ```

2. **Partial Rollback Options:**
   - Revert only Magick.NET if blocking
   - Keep framework at net10.0 but use older package versions temporarily
   - Use multi-targeting if needed: `<TargetFrameworks>net5.0;net10.0</TargetFrameworks>`

3. **Investigation Period:**
   - Keep net5.0 working in separate branch
   - Debug issues in upgrade branch
   - Merge only when fully validated

---

## Success Criteria

### The upgrade is complete and successful when:

#### Technical Criteria
- [x] Project targets .NET 10.0 (.NETCoreApp,Version=v10.0)
- [x] All 6 packages updated to target versions
- [x] Magick.NET security vulnerability resolved (version 14.10.2)
- [x] Solution builds with 0 errors
- [x] Solution builds with 0 warnings (or documented acceptable warnings)
- [x] No package dependency conflicts
- [x] dotnet restore succeeds
- [x] dotnet build succeeds

#### Functional Criteria
- [x] Application starts successfully
- [x] React SPA loads and renders
- [x] Album scanning background service works
- [x] Spotify API integration functions
- [x] Cover art image processing works correctly
- [x] Database operations succeed
- [x] All API endpoints respond correctly
- [x] SignalR hubs connect properly

#### Quality Criteria
- [x] All existing tests pass (if tests exist)
- [x] No runtime errors in critical paths
- [x] Performance within acceptable range
- [x] No new security vulnerabilities introduced
- [x] Code quality maintained or improved

#### Documentation Criteria
- [x] Upgrade changes documented
- [x] Any breaking changes noted
- [x] Known issues documented
- [x] Testing results recorded

---

## Implementation Timeline

### Estimated Schedule

**Total Estimated Time**: 2-4 hours

**Phase Breakdown:**

1. **Preparation** (15 minutes)
   - Review plan
   - Ensure clean working directory
   - Create backup/branch
   - Verify baseline build

2. **Project File Updates** (15 minutes)
   - Update TargetFramework
   - Update all PackageReferences
   - Save changes

3. **Build and Fix** (1-2 hours)
   - Run dotnet restore
   - Run dotnet build
   - Fix compilation errors
   - Address Magick.NET API changes
   - Resolve warnings

4. **Testing** (1-1.5 hours)
   - Run unit tests
   - Manual functional testing
   - Test all critical paths
   - Performance validation

5. **Documentation** (15 minutes)
   - Document changes made
   - Note any issues encountered
   - Record test results

**Milestone**: Upgrade Complete and Validated

---

## Validation Checklist

### Pre-Upgrade
- [ ] Current solution builds successfully on .NET 5.0
- [ ] All existing tests pass (baseline)
- [ ] Clean working directory (no uncommitted changes)
- [ ] Created upgrade branch
- [ ] Documented current build warnings (if any)

### During Upgrade
- [ ] TargetFramework updated to net10.0
- [ ] All 6 packages updated to target versions
- [ ] dotnet restore completed successfully
- [ ] No package dependency conflicts

### Post-Upgrade Build
- [ ] dotnet build succeeds with 0 errors
- [ ] Build warnings addressed or documented
- [ ] No package restore warnings
- [ ] Project file changes reviewed

### Testing
- [ ] Unit tests executed and passing
- [ ] Application starts without errors
- [ ] React SPA loads correctly
- [ ] Album scanning tested
- [ ] Image processing tested
- [ ] Database operations tested
- [ ] API endpoints tested
- [ ] Performance validated

### Final Review
- [ ] No security vulnerabilities remain
- [ ] All success criteria met
- [ ] Changes documented
- [ ] Ready for code review
- [ ] Ready for merge

---

## Additional Considerations

### .NET SDK Requirements

Ensure .NET 10.0 SDK is installed:

```bash
dotnet --list-sdks
```

If not installed, download from: https://go.microsoft.com/fwlink/?linkid=2313714

### Database Migrations

**Entity Framework Core Migrations:**
- Existing migrations should remain compatible
- May need to regenerate migrations if EF Core schema changes
- Test migration apply/rollback after upgrade

**SQLite Database:**
- SQLite database files are version-agnostic
- No database upgrade required
- Test all CRUD operations

### React SPA Considerations

**Frontend Dependencies:**
- npm packages are independent of .NET version
- No frontend package updates required
- Verify dev server proxy still works
- Test production build process

### Background Services

**AlbumScanner Service:**
- Hosted service pattern remains compatible
- Test background task execution
- Verify HttpClient injection still works
- Check Spotify API calls function correctly

### SignalR Hubs

**Real-time Communication:**
- SignalR protocol compatible across .NET versions
- Test hub connections
- Verify client-server communication
- Check browser console for errors

---

## References

### Official Documentation
- [.NET 10.0 Release Notes](https://github.com/dotnet/core/tree/main/release-notes/10.0)
- [ASP.NET Core 10.0 Breaking Changes](https://learn.microsoft.com/aspnet/core/migration/9.0-to-10.0)
- [EF Core 10.0 What's New](https://learn.microsoft.com/ef/core/what-is-new/ef-core-10.0/whatsnew)
- [Magick.NET Documentation](https://github.com/dlemstra/Magick.NET/releases)

### Migration Guides
- [Migrate from .NET 5.0 to 6.0](https://learn.microsoft.com/aspnet/core/migration/50-to-60)
- [Migrate from .NET 6.0 to 7.0](https://learn.microsoft.com/aspnet/core/migration/60-to-70)
- [Migrate from .NET 7.0 to 8.0](https://learn.microsoft.com/aspnet/core/migration/70-to-80)
- [Migrate from .NET 8.0 to 9.0](https://learn.microsoft.com/aspnet/core/migration/80-to-90)
- [Migrate from .NET 9.0 to 10.0](https://learn.microsoft.com/aspnet/core/migration/9.0-to-10.0)

---

## Conclusion

This upgrade plan provides a clear, step-by-step path to migrate the Covers solution from .NET 5.0 to .NET 10.0. The all-at-once strategy is appropriate given the single-project structure and low complexity. 

**Key Success Factors:**
1. Address Magick.NET security vulnerability immediately
2. Thorough testing of image processing functionality
3. Validation of all critical user paths
4. Clean build with no warnings

The execution stage will follow this plan systematically, validating each step before proceeding.

**Next Step**: Proceed to Execution stage to implement this plan and apply all changes.

---

*This plan supports the systematic execution of the .NET 10 upgrade for the Covers solution.*
