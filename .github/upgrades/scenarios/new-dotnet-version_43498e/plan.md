# .NET 10 Upgrade Plan for Covers Solution

**Date**: 2026-02-17  
**Strategy**: All-At-Once  
**Current Framework**: .NET 5.0  
**Target Framework**: .NET 10.0 (Long Term Support)

---

## Executive Summary

This plan outlines the upgrade of the Covers solution from .NET 5.0 to .NET 10.0. The solution consists of a single ASP.NET Core project with 3,099 lines of code across 51 files. The assessment identified 6 package upgrades needed, including 1 security vulnerability that will be addressed. Given the single-project structure and low complexity (Low difficulty rating), an all-at-once upgrade strategy is optimal.

**Key Highlights:**
- Single ASP.NET Core project upgrade
- 6 package updates required (including security fix for Magick.NET)
- No API compatibility issues identified
- Estimated impact: 0+ lines of code modifications
- All-at-once execution for fastest completion

---

## Upgrade Strategy: All-At-Once

### Rationale

The all-at-once strategy is selected because:

1. **Single Project**: Only one project (Covers.csproj) requires upgrade
2. **Simple Dependencies**: Clear dependency structure with no inter-project dependencies
3. **Small Codebase**: 3,099 lines of code is manageable for simultaneous upgrade
4. **Low Complexity**: Assessment rated difficulty as 🟢 Low
5. **Modern Base**: Already SDK-style project on .NET 5.0, upgrading to .NET 10.0
6. **Clear Package Compatibility**: All packages have identified upgrade paths

### Strategy Characteristics

- Update project file to target net10.0
- Upgrade all 6 packages simultaneously
- Single build and test validation phase
- Faster total completion time
- Lower coordination overhead

---

## Dependency Analysis

### Project Structure

```
Covers.sln
└── Covers.csproj (ASP.NET Core, SDK-style)
    - Current: net5.0
    - Target: net10.0
    - Type: AspNetCore
    - LOC: 3,099
    - Files: 51
```

### Upgrade Order

Since this is a single-project solution, there is no dependency ordering required. The upgrade will proceed as follows:

**Phase 1: Single Project Upgrade**
- Covers.csproj (AspNetCore application)

---

## Package Update Strategy

### Package Upgrade Overview

| Status | Count | Percentage |
|--------|-------|------------|
| ✅ Compatible (No Update) | 4 | 40.0% |
| 🔄 Upgrade Recommended | 6 | 60.0% |
| **Total** | **10** | **100%** |

### Detailed Package Updates

#### Critical Security Update

| Package | Current | Target | Reason |
|---------|---------|--------|--------|
| Magick.NET-Q8-AnyCPU | 7.22.3 | 14.10.2 | 🔴 **Security Vulnerability** |

**Note**: This package has a known security vulnerability and must be upgraded immediately.

#### Recommended Package Upgrades

| Package | Current | Target | Reason |
|---------|---------|--------|--------|
| Microsoft.AspNetCore.SpaServices.Extensions | 5.0.1 | 10.0.3 | Framework compatibility |
| Microsoft.EntityFrameworkCore.Design | 5.0.1 | 10.0.3 | Framework compatibility |
| Microsoft.EntityFrameworkCore.Proxies | 5.0.1 | 10.0.3 | Framework compatibility |
| Microsoft.EntityFrameworkCore.Sqlite | 5.0.1 | 10.0.3 | Framework compatibility |
| Microsoft.EntityFrameworkCore.Tools | 5.0.1 | 10.0.3 | Framework compatibility |

#### Compatible Packages (No Update Required)

| Package | Current Version | Status |
|---------|----------------|--------|
| NSwag.AspNetCore | 13.9.4 | ✅ Compatible |
| SpotifyAPI.Web | 6.0.0 | ✅ Compatible |
| SpotifyAPI.Web.Auth | 6.0.0 | ✅ Compatible |
| TagLibSharp | 2.2.0 | ✅ Compatible |

---

## Framework Upgrade Considerations

### Target Framework Assignment

- **Covers.csproj**: net5.0 → net10.0

### Expected Breaking Changes

Based on the .NET 5.0 to .NET 10.0 upgrade path, the following areas should be monitored:

1. **ASP.NET Core Changes**
   - Minimal hosting model updates (if using)
   - Middleware registration patterns
   - Configuration system changes
   - Authentication/authorization updates

2. **Entity Framework Core Updates**
   - EF Core 5.0.1 → 10.0.3 may include:
     - Query translation improvements
     - New migration patterns
     - Updated interceptor patterns
   - Database migrations may need regeneration

3. **API Changes**
   - Assessment shows 0 API compatibility issues identified
   - Runtime behavior changes should still be validated through testing

4. **Package-Specific Changes**
   - Magick.NET upgrade (7.22.3 → 14.10.2) is a major version jump
     - Review API changes in Magick.NET documentation
     - Test image processing functionality thoroughly
   - SpaServices.Extensions may have deprecations or changes

---

## Project-Specific Upgrade Plans

### Covers.csproj (AspNetCore Application)

**Current State:**
- Target Framework: net5.0
- Project Type: ASP.NET Core Web Application
- SDK Style: Yes
- Lines of Code: 3,099
- Files: 51
- Dependencies: 10 NuGet packages

**Proposed Changes:**

1. **Update Project File (Covers.csproj)**
   ```xml
   <TargetFramework>net10.0</TargetFramework>
   ```

2. **Update Package References**
   - Magick.NET-Q8-AnyCPU: 7.22.3 → 14.10.2
   - Microsoft.AspNetCore.SpaServices.Extensions: 5.0.1 → 10.0.3
   - Microsoft.EntityFrameworkCore.Design: 5.0.1 → 10.0.3
   - Microsoft.EntityFrameworkCore.Proxies: 5.0.1 → 10.0.3
   - Microsoft.EntityFrameworkCore.Sqlite: 5.0.1 → 10.0.3
   - Microsoft.EntityFrameworkCore.Tools: 5.0.1 → 10.0.3

3. **Code Modifications (To Be Discovered)**
   - Changes will be identified during compilation
   - Focus areas:
     - Image processing code using Magick.NET
     - Entity Framework DbContext and migrations
     - ASP.NET Core middleware configuration
     - SPA service integration

4. **Configuration Updates**
   - Review appsettings.json for any framework-specific settings
   - Verify Startup.cs or Program.cs configuration patterns
   - Check launchSettings.json for compatibility

**Risk Level**: 🟢 Low
- Small codebase with clear upgrade path
- Good package compatibility
- Modern SDK-style project already

**Validation Steps:**
- [ ] Project builds without errors
- [ ] Project builds without warnings
- [ ] No package dependency conflicts
- [ ] All unit tests pass (if present)
- [ ] Application starts successfully
- [ ] Image processing functionality works correctly
- [ ] Database operations function properly
- [ ] SPA integration works as expected
- [ ] API endpoints respond correctly

---

## Code Changes Planning

### Expected Modification Areas

1. **Magick.NET API Changes (High Priority)**
   - Major version upgrade (7.x → 14.x)
   - Potential API surface changes
   - Property type changes (based on repository memory: uint for Width/Height)
   - Files likely affected: Controllers/CoverController.cs, Services/SpotifyService.cs, BackgroundServices/AlbumScanner.cs

2. **Entity Framework Core Updates**
   - Migration files may need regeneration
   - DbContext configuration patterns
   - Query syntax updates

3. **ASP.NET Core Configuration**
   - Program.cs/Startup.cs updates
   - Service registration patterns
   - Middleware pipeline configuration

4. **Obsolete API Replacements**
   - Repository memory indicates WebClient was replaced with HttpClient in previous upgrade
   - Verify no obsolete APIs remain

### Discovery-Driven Approach

Since the assessment identified 0 API compatibility issues with certainty, code changes will be discovered through:
1. Compilation after framework update
2. Package update compatibility checks
3. Runtime testing and validation

---

## Testing Strategy

### Multi-Level Testing Approach

#### Project-Level Testing

After upgrading Covers.csproj:
- **Build Validation**
  - [ ] Restore packages successfully
  - [ ] Build completes without errors
  - [ ] Build completes without warnings
  - [ ] No dependency version conflicts

- **Unit Testing**
  - [ ] All existing unit tests pass
  - [ ] Test coverage maintained
  - [ ] No new test failures introduced

- **Integration Testing**
  - [ ] Application starts without errors
  - [ ] Database connection successful
  - [ ] Entity Framework migrations work
  - [ ] API endpoints respond correctly

#### Functional Testing

Critical functionality to validate:
- [ ] **Image Processing (Magick.NET)**
  - Album cover generation
  - Image resizing and formatting
  - Cover art retrieval and processing
  
- [ ] **Database Operations (EF Core)**
  - CRUD operations work correctly
  - Proxies and lazy loading function
  - SQLite database access successful
  
- [ ] **Spotify Integration**
  - API authentication works
  - Album data retrieval successful
  - Cover art downloads function
  
- [ ] **SPA Services**
  - Frontend application loads
  - API integration works
  - Static file serving correct

#### Performance Testing
- [ ] Application startup time comparable
- [ ] API response times acceptable
- [ ] Image processing performance maintained
- [ ] Memory usage within expected range

#### Security Validation
- [ ] No security vulnerabilities in upgraded packages
- [ ] Authentication/authorization working
- [ ] HTTPS configuration correct
- [ ] API security maintained

---

## Risk Assessment

### Risk Levels by Category

#### High Priority Risks

**1. Magick.NET Major Version Upgrade (7.22.3 → 14.10.2)**
- **Risk Level**: 🔴 High
- **Impact**: Breaking changes in image processing API
- **Likelihood**: High (major version jump)
- **Mitigation**:
  - Review Magick.NET changelog for breaking changes
  - Test all image processing code paths thoroughly
  - Have rollback plan ready
  - Repository memory indicates uint type changes for Width/Height properties
- **Affected Files**: Controllers/CoverController.cs, Services/SpotifyService.cs, BackgroundServices/AlbumScanner.cs

#### Medium Priority Risks

**2. Entity Framework Core Upgrade (5.0.1 → 10.0.3)**
- **Risk Level**: 🟡 Medium
- **Impact**: Database query behavior changes
- **Likelihood**: Medium
- **Mitigation**:
  - Test all database operations
  - Verify proxy functionality
  - Check migration compatibility
  - Backup database before testing

**3. ASP.NET Core Framework Changes**
- **Risk Level**: 🟡 Medium
- **Impact**: Middleware or configuration incompatibilities
- **Likelihood**: Low-Medium
- **Mitigation**:
  - Review ASP.NET Core migration guides
  - Test application startup
  - Validate all middleware functionality

#### Low Priority Risks

**4. SpaServices.Extensions Compatibility**
- **Risk Level**: 🟢 Low
- **Impact**: Frontend integration issues
- **Likelihood**: Low
- **Mitigation**:
  - Test SPA loading and routing
  - Verify static file serving
  - Check development/production modes

### Overall Risk Profile

- **Project Complexity**: Low (single project, SDK-style)
- **Codebase Size**: Small (3,099 LOC)
- **Test Coverage**: To be determined
- **Business Criticality**: Medium (production application)
- **Rollback Feasibility**: High (Git-based, single project)

---

## Timeline and Effort Estimation

### Phase Breakdown

| Phase | Duration | Effort | Dependencies |
|-------|----------|--------|--------------|
| **Preparation** | 15 min | 0.25h | Assessment complete |
| Update TargetFramework | 5 min | 0.1h | None |
| Update Package References | 10 min | 0.15h | TargetFramework updated |
| **Build & Resolve Issues** | 1-2 hours | 1.5h | Packages updated |
| Code Modifications | 30-90 min | 1h | Build errors identified |
| **Testing** | 2-3 hours | 2.5h | Code complete |
| Unit Testing | 30 min | 0.5h | Build successful |
| Integration Testing | 1 hour | 1h | Unit tests pass |
| Functional Testing | 1 hour | 1h | Integration tests pass |
| **Documentation & Review** | 30 min | 0.5h | Testing complete |
| **Total** | **4-6 hours** | **5h** | |

### Assumptions

- No major undiscovered breaking changes
- Test infrastructure already exists
- Development environment properly configured
- .NET 10 SDK installed
- Single developer execution

### Buffer Time

- 20% buffer added for unexpected issues: **1 hour**
- **Total with Buffer**: 5-7 hours

---

## Rollback Strategy

### Rollback Triggers

Rollback should be initiated if:
- Critical functionality breaks and cannot be quickly fixed
- Security vulnerabilities introduced
- Performance degradation exceeds 50%
- Build cannot be stabilized within 3 hours
- Database migrations fail irreversibly

### Rollback Steps

1. **Git-Based Rollback** (Primary)
   ```bash
   git reset --hard <previous-commit>
   git clean -fd
   ```

2. **Package Restore**
   ```bash
   dotnet restore
   ```

3. **Verification**
   - Build application
   - Run tests
   - Verify functionality

4. **Database Rollback** (If migrations applied)
   - Restore database backup
   - Or use EF Core migration rollback commands

### Prevention Measures

- Create Git branch for upgrade work (already on: copilot/upgrade-covers-sln-to-net-10)
- Commit frequently with clear messages
- Backup database before testing
- Tag release point before upgrade begins
- Document all changes made

---

## Success Criteria

The upgrade is considered complete and successful when ALL of the following criteria are met:

### Technical Criteria

- [ ] **Framework Update Complete**
  - Covers.csproj targets net10.0
  - No multi-targeting required
  
- [ ] **Package Updates Applied**
  - All 6 recommended package updates installed
  - No package version conflicts
  - Security vulnerability in Magick.NET resolved
  
- [ ] **Build Success**
  - Solution builds without errors
  - Solution builds without warnings
  - All dependencies resolve correctly
  
- [ ] **Code Quality**
  - No obsolete API usage warnings
  - No deprecated pattern warnings
  - Code analysis passes
  
- [ ] **Testing Complete**
  - All unit tests pass
  - All integration tests pass
  - Functional testing complete
  - Performance benchmarks met
  
- [ ] **Security Validated**
  - No NuGet package vulnerabilities
  - Security scan passes
  - Authentication/authorization working

### Functional Criteria

- [ ] **Application Functionality**
  - Application starts successfully
  - All API endpoints functional
  - SPA loads and operates correctly
  
- [ ] **Image Processing**
  - Cover art generation works
  - Image manipulation functions correctly
  - No quality degradation
  
- [ ] **Database Operations**
  - All CRUD operations work
  - Entity Framework proxies functional
  - SQLite connection stable
  
- [ ] **Spotify Integration**
  - API authentication successful
  - Album data retrieval works
  - Cover downloads functional

### Documentation Criteria

- [ ] **Code Documentation**
  - Breaking changes documented
  - Migration notes created
  - Known issues logged
  
- [ ] **Deployment Notes**
  - Deployment steps documented
  - Environment requirements listed
  - Configuration changes noted

---

## Execution Checklist

This checklist will guide the Execution stage:

### Pre-Execution
- [ ] Verify on correct branch (copilot/upgrade-covers-sln-to-net-10)
- [ ] Backup current database
- [ ] Tag current commit for easy rollback
- [ ] Verify .NET 10 SDK installed

### Phase 1: Framework and Package Updates
- [ ] Update Covers.csproj TargetFramework to net10.0
- [ ] Update Magick.NET-Q8-AnyCPU to 14.10.2
- [ ] Update Microsoft.AspNetCore.SpaServices.Extensions to 10.0.3
- [ ] Update Microsoft.EntityFrameworkCore.Design to 10.0.3
- [ ] Update Microsoft.EntityFrameworkCore.Proxies to 10.0.3
- [ ] Update Microsoft.EntityFrameworkCore.Sqlite to 10.0.3
- [ ] Update Microsoft.EntityFrameworkCore.Tools to 10.0.3
- [ ] Run dotnet restore
- [ ] Commit: "Update target framework to net10.0 and upgrade packages"

### Phase 2: Build and Fix Issues
- [ ] Run dotnet build
- [ ] Resolve compilation errors
- [ ] Address obsolete API warnings
- [ ] Fix Magick.NET API changes (Width/Height uint conversions)
- [ ] Update code for any breaking changes
- [ ] Commit: "Fix compilation errors and breaking changes"

### Phase 3: Testing
- [ ] Run unit tests
- [ ] Fix failing tests
- [ ] Run integration tests
- [ ] Test image processing functionality
- [ ] Test database operations
- [ ] Test Spotify API integration
- [ ] Test SPA functionality
- [ ] Verify no security vulnerabilities
- [ ] Commit: "Fix test failures and verify functionality"

### Phase 4: Final Validation
- [ ] Full solution build (clean build)
- [ ] All tests passing
- [ ] Application runs successfully
- [ ] Performance acceptable
- [ ] Security scan clean
- [ ] Documentation updated
- [ ] Commit: "Complete .NET 10 upgrade"

### Post-Execution
- [ ] Create pull request
- [ ] Request code review
- [ ] Update deployment documentation
- [ ] Plan production deployment

---

## Breaking Changes Catalog

### Known Breaking Changes (From Assessment)

None identified by automated analysis. However, based on the upgrade path and repository memory:

### .NET 5.0 to .NET 10.0 Framework Changes

1. **WebClient Obsolescence** (Already Addressed)
   - Previous upgrade replaced WebClient with HttpClient
   - No action needed

### Magick.NET 7.22.3 to 14.10.2 Changes

1. **Property Type Changes** (From Repository Memory)
   - `MagickGeometry.Width` and `MagickGeometry.Height` changed from `int` to `uint`
   - **Affected Files**:
     - Controllers/CoverController.cs:75
     - Services/SpotifyService.cs:187
     - BackgroundServices/AlbumScanner.cs:412
   - **Fix**: Cast or update variable types to uint

2. **Potential API Changes**
   - Review Magick.NET 14.x changelog
   - Test all image operations
   - Verify constructor signatures

### Entity Framework Core 5.0.1 to 10.0.3 Changes

1. **Query Translation**
   - Improved LINQ query translation
   - Some queries may translate differently
   - Test all database queries

2. **Migration Patterns**
   - Verify migration compatibility
   - May need to regenerate migrations

### ASP.NET Core 5.0 to 10.0 Changes

1. **Minimal Hosting Model** (If Applicable)
   - Check Program.cs patterns
   - Verify middleware registration

2. **Configuration Patterns**
   - Service lifetime specifications
   - Dependency injection updates

---

## Dependencies and Prerequisites

### Development Environment

- [ ] .NET 10.0 SDK installed
- [ ] Visual Studio 2022 (version 17.10+) or VS Code with C# extension
- [ ] Git installed and configured
- [ ] Database tools (for SQLite)

### System Requirements

- Windows, macOS, or Linux with .NET 10 support
- Minimum 4 GB RAM
- 2 GB free disk space

### External Dependencies

- Spotify API credentials (existing configuration should work)
- Database files (SQLite)
- Any external services the application depends on

---

## Notes and Considerations

### Special Considerations

1. **Security Fix Priority**
   - Magick.NET upgrade addresses known security vulnerability
   - Should be deployed to production as soon as validated

2. **Image Processing Library**
   - Magick.NET is core to application functionality
   - Thorough testing of all image operations required
   - Performance comparison before/after upgrade recommended

3. **Entity Framework Core**
   - Major version upgrade may affect query behavior
   - Review EF Core 10.0 release notes
   - Test lazy loading with proxies

4. **SPA Services**
   - Microsoft.AspNetCore.SpaServices.Extensions may be deprecated
   - Consider future migration to alternative SPA hosting
   - Current upgrade maintains existing pattern

### Future Considerations

1. **Post-Upgrade Optimizations**
   - Explore .NET 10 performance improvements
   - Consider adopting new C# language features
   - Review newer ASP.NET Core patterns

2. **Dependency Monitoring**
   - Set up automated dependency update checks
   - Monitor for future security vulnerabilities
   - Plan regular upgrade cycles

3. **Migration to Modern Patterns**
   - Consider eventual migration from SpaServices.Extensions
   - Explore new .NET 10 features
   - Update to latest best practices

---

## Appendix

### Reference Documentation

- [.NET 10 Release Notes](https://docs.microsoft.com/en-us/dotnet/core/whats-new/dotnet-10)
- [ASP.NET Core 10.0 Migration Guide](https://docs.microsoft.com/en-us/aspnet/core/migration/50-to-60)
- [Entity Framework Core 10.0 Release Notes](https://docs.microsoft.com/en-us/ef/core/what-is-new/ef-core-10.0/whatsnew)
- [Magick.NET Documentation](https://github.com/dlemstra/Magick.NET)

### Assessment Reference

- Assessment File: `.github/upgrades/scenarios/new-dotnet-version_43498e/assessment.md`
- Assessment Date: 2026-02-17
- Total Projects Analyzed: 1
- Total Issues Identified: 12

---

*This plan provides a complete blueprint for the Execution stage, where these changes will be systematically applied and validated.*
