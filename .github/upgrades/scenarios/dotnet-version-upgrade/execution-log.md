
## [2026-03-09 07:11] 01-tfm-and-packages

Updated TargetFramework from net5.0 to net10.0. Updated all NuGet packages: Magick.NET-Q8-AnyCPU (7.22.3→14.10.3), Microsoft.AspNetCore.SpaServices.Extensions (5.0.1→10.0.3), all EF Core packages (5.0.1→10.0.3), NSwag.AspNetCore (13.9.4→14.6.3), SpotifyAPI.Web and SpotifyAPI.Web.Auth (6.0.0→7.4.2), TagLibSharp (2.2.0→2.3.0).


## [2026-03-09 07:13] 02-code-fixes

Fixed all code issues for net10.0 compatibility:
1. CoverController.cs: Added (uint) casts for MagickGeometry Width/Height properties (Magick.NET v14 breaking change)
2. Startup.cs: Replaced UseSwaggerUi3() with UseSwaggerUi() (NSwag 14.x API change); Added services.AddHttpClient() registration
3. SpotifyService.cs: Replaced System.Net.WebClient with System.Net.Http.HttpClient via IHttpClientFactory; Added IHttpClientFactory constructor parameter; Fixed MagickGeometry Width to use uint literal (800u)
4. CoverDownloadService.cs: Fixed MagickGeometry Width to use uint literal (800u)
5. AlbumScanner.cs: Replaced System.Net.WebClient with HttpClient via IHttpClientFactory from service provider; Fixed MagickGeometry Width to use uint literal (800u)
Build result: 0 errors, 0 warnings.


## [2026-03-09 07:14] 03-build-validate

Final validation: Clean build with dotnet clean followed by dotnet build. Result: Build succeeded, 0 errors, 0 warnings. The solution successfully targets net10.0. All package updates applied and all breaking API changes resolved.

