# .NET Version Upgrade Plan

**Generated**: 2026-03-09
**Solution**: D:\a\Covers\Covers\Covers.sln
**Upgrade**: net5.0 → net10.0

### Selected Strategy
**All-At-Once** — All projects upgraded simultaneously in a single operation.
**Rationale**: 1 project (Covers), low complexity, clear dependency structure.

## Projects

| Project | Type | Current | Target |
|---------|------|---------|--------|
| Covers.csproj | AspNetCore WebApp | net5.0 | net10.0 |

## Tasks

| ID | Description | Status |
|----|-------------|--------|
| 01-tfm-and-packages | Update target framework and NuGet packages | ⬜ Pending |
| 02-code-fixes | Fix code issues for net10.0 compatibility | ⬜ Pending |
| 03-build-validate | Build solution and verify 0 errors/warnings | ⬜ Pending |

## Package Updates

| Package | Current | Target |
|---------|---------|--------|
| Magick.NET-Q8-AnyCPU | 7.22.3 | 14.10.3 |
| Microsoft.AspNetCore.SpaServices.Extensions | 5.0.1 | 10.0.3 |
| Microsoft.EntityFrameworkCore.Design | 5.0.1 | 10.0.3 |
| Microsoft.EntityFrameworkCore.Proxies | 5.0.1 | 10.0.3 |
| Microsoft.EntityFrameworkCore.Sqlite | 5.0.1 | 10.0.3 |
| Microsoft.EntityFrameworkCore.Tools | 5.0.1 | 10.0.3 |
| NSwag.AspNetCore | 13.9.4 | 14.6.3 |
| SpotifyAPI.Web | 6.0.0 | 7.4.2 |
| SpotifyAPI.Web.Auth | 6.0.0 | 7.4.2 |
| TagLibSharp | 2.2.0 | 2.3.0 |

## Known Code Changes Required

1. **WebClient → HttpClient**: Replace `WebClient` usages in `SpotifyService.cs` and `AlbumScanner.cs`
2. **MagickGeometry Width/Height**: Add `uint` casts (Magick.NET v14 change)
3. **NSwag API**: `UseSwaggerUi3()` → `UseSwaggerUi()`
4. **HttpClient registration**: Add `services.AddHttpClient()` in `Startup.cs`
