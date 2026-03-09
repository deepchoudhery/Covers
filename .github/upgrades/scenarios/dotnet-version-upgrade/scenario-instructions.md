# Scenario Instructions: .NET Version Upgrade

## Scenario Parameters

- **Target Framework**: net10.0
- **Solution**: D:\a\Covers\Covers\Covers.sln
- **Project**: D:\a\Covers\Covers\Covers.csproj
- **Source Framework**: net5.0

## Flow Mode
**Automatic** - Run end-to-end without pausing for confirmation.

## Strategy
**Selected**: All-at-Once
**Rationale**: Single project solution with straightforward package upgrades and limited code changes. Low complexity.

### Execution Constraints
- Single atomic upgrade — all projects updated together
- Validate full solution build after upgrade
- Fix all warnings to ensure 0 errors and 0 warnings

## User Preferences

### Technical Preferences
- Magick.NET v14 requires uint casts for MagickGeometry Width/Height properties
- WebClient usages need to be replaced with HttpClient
- HttpClient should be registered via services.AddHttpClient() in Startup.cs
- Microsoft packages should be updated to 10.0.x versions
- Magick.NET should be updated to 14.x.x (Magick.NET-Q8-AnyCPU)
- SpotifyAPI.Web should be updated to compatible version (7.4.2)
- NSwag 14.x uses UseSwaggerUi() instead of UseSwaggerUi3()

### Execution Style
- Automatic mode: no pausing
- 0 errors and 0 warnings required

## Key Decisions Log
- 2026-03-09: Upgrade from net5.0 to net10.0
- 2026-03-09: All-at-Once strategy selected (single project)
- 2026-03-09: Package versions: Magick.NET=14.10.3, EF Core=10.0.3, SpaServices=10.0.3, NSwag=14.6.3, SpotifyAPI.Web=7.4.2, TagLibSharp=2.3.0
