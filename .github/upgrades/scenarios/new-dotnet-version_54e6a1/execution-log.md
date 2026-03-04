
## [2026-03-04 09:56] TASK-001: Update framework target and NuGet packages

Status: Complete

- **Files Modified**: Covers.csproj, Controllers/CoverController.cs, Services/SpotifyService.cs, BackgroundServices/AlbumScanner.cs, Startup.cs
- **Code Changes**: 
  - Updated TargetFramework from net5.0 to net10.0
  - Updated 6 NuGet packages (Magick.NET 7.22.3→14.10.3, EF Core 5.0.1→10.0.3 x4, SpaServices 5.0.1→10.0.3)
  - Fixed MagickGeometry Width/Height int→uint cast in CoverController.cs
  - Replaced WebClient with IHttpClientFactory+HttpClient in SpotifyService and AlbumScanner
  - Added services.AddHttpClient() to Startup.cs
  - Removed obsolete System.Net using directives
- **Errors Fixed**: CS0266 (uint cast), SYSLIB0014 (WebClient obsolete)
- **Tests**: Build succeeds with 0 errors and 0 warnings

Complete - All three tasks accomplished together during TASK-001 and TASK-002 execution

