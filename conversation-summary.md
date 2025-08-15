# Project Conversation Summary

## Goal
Secure an ASP.NET Core backend with Azure AD authentication and scope-based authorization, supporting only personal Microsoft accounts (MSA).

## Steps Covered
1. **Azure AD App Registration**
   - Created for MSA only.
   - Exposed API scope (Weather.Read).
   - Enabled public client flows.
   - Set up authentication and redirect URI.
2. **Backend Code & Config**
   - Updated `appsettings.json` and `Program.cs` for AAD integration.
   - Used Microsoft.Identity.Web and JwtBearer.
3. **Tools Installed**
   - NuGet packages: Microsoft.Identity.Web, Microsoft.AspNetCore.Authentication.JwtBearer.
   - VS Code REST Client extension.
   - jq for JSON parsing in terminal.
4. **Authentication Flow**
   - Used device code flow (password grant unsupported for MSA).
   - Parameterized `.http` file for automated token usage.
5. **Troubleshooting**
   - Invalid scope: used full scope URI.
   - Unsupported grant: switched to device code flow.
   - Client secret required: enabled public client flows.
   - Local endpoint issues: matched port and protocol to port forwarding.
6. **Documentation**
   - All steps, troubleshooting, and local testing documented in README.
7. **Git & GitHub**
   - Initialized git, created branches, pushed code to `dotnet-aad-starter` repo.

## External Changes
- Azure Portal: App registration, API exposure, authentication settings.
- Terminal: Installed jq, used curl for token requests, git commands.
- VS Code: Installed REST Client extension.

## Local Testing
- Used port forwarding (8080 for backend).
- Used device code flow and REST Client for secure API calls.

---
This summary can be reused for onboarding, troubleshooting, or extending similar projects.
