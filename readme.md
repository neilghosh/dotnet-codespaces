# Securing the Backend with Azure AD (Personal Microsoft Accounts)

## Step-by-Step Guide

### 1. Create and Configure Azure AD App Registration
1. Go to Azure Portal > Azure Active Directory > App registrations > New registration.
2. Set Supported account types to **Personal Microsoft accounts only**.
3. Set a name (e.g., "Weather API App").
4. After creation, note the **Application (client) ID**.
5. Under **Expose an API**, set the Application ID URI (e.g., `api://<client_id>`).
6. Add a scope (e.g., `Weather.Read`).
7. Under **Authentication**, add a redirect URI for public clients: `https://login.microsoftonline.com/common/oauth2/nativeclient`.
8. Enable **Allow public client flows**.

### 2. Update Backend Code and Configuration
* Install NuGet packages:
	- `Microsoft.Identity.Web`
	- `Microsoft.AspNetCore.Authentication.JwtBearer`
* In `appsettings.json` and `appsettings.Development.json`, set:
	- `Instance`: `https://login.microsoftonline.com/`
	- `TenantId`: `consumers`
	- `ClientId`: `<your client id>`
	- `Audience`: `<your client id>`
* In `Program.cs`, configure authentication and protect endpoints with scope-based authorization (e.g., `Weather.Read`).

### 3. Install Tools
* VS Code REST Client extension (for `.http` file testing)
* jq (for parsing JSON in terminal)

### 4. Test Authentication and API Locally
#### Device Code Flow (Recommended for MSA)
1. Request device code:
	 ```http
	 POST https://login.microsoftonline.com/consumers/oauth2/v2.0/devicecode
	 Content-Type: application/x-www-form-urlencoded

	 client_id=<your client id>&scope=api://<your client id>/Weather.Read
	 ```
2. Open the `verification_uri` in your browser, enter the `user_code`, and sign in.
3. Poll for access token:
	 ```http
	 POST https://login.microsoftonline.com/consumers/oauth2/v2.0/token
	 Content-Type: application/x-www-form-urlencoded

	 grant_type=device_code&client_id=<your client id>&device_code=<device_code>
	 ```
4. Use the access token to call your API:
	 ```http
	 GET http://localhost:8080/weatherforecast
	 Authorization: Bearer <access_token>
	 ```

#### Parameterize Variables in .http File
You can use REST Client variables and response scripts to automate token passing:
```http
@accessToken = <your_token>
GET http://localhost:8080/weatherforecast
Authorization: Bearer {{accessToken}}
```
Or extract from previous response:
```http
> {% client.global.set('accessToken', response.body.access_token); %}
```

### 5. Troubleshooting
* **AADSTS700016: Application not found**
	- Ensure you use the Application (client) ID, not the object ID.
	- Double-check config files and portal registration.
* **Invalid scope**
	- Use the full scope URI: `api://<client_id>/Weather.Read`.
	- Scope must match exactly as defined in Azure portal.
* **grant_type unsupported**
	- Password grant is not supported for MSA. Use device code or authorization code flow.
* **AADSTS70002: client_secret required**
	- Enable public client flows in Azure portal.
	- Add a native client redirect URI.
* **Local endpoint issues**
	- Use the correct port (e.g., 8080) and protocol (http/https) as per your port forwarding setup.

### 6. External Changes Required
* Azure Portal: App registration, API exposure, authentication settings.
* Terminal: Install jq, use curl for manual token requests.
* VS Code: Install REST Client extension.

### 7. Local Testing Summary
* Start backend and frontend in VS Code.
* Use port forwarding (e.g., 8080 for backend).
* Use device code flow to acquire token and test API endpoint locally.

---
# GitHub Codespaces ♥️ .NET

Want to try out the latest performance improvements coming with .NET for web development? 

This repo builds a Weather API, OpenAPI integration to test with [Scalar](https://learn.microsoft.com/aspnet/core/fundamentals/openapi/using-openapi-documents?view=aspnetcore-9.0#use-scalar-for-interactive-api-documentation), and displays the data in a web application using Blazor with .NET. 

We've given you both a frontend and backend to play around with and where you go from here is up to you!

Everything you do here is contained within this one codespace. There is no repository on GitHub yet. If and when you’re ready you can click "Publish Branch" and we’ll create your repository and push up your project. If you were just exploring then and have no further need for this code then you can simply delete your codespace and it's gone forever.

### Run Options

[![Open in GitHub Codespaces](https://img.shields.io/static/v1?style=for-the-badge&label=GitHub+Codespaces&message=Open&color=lightgrey&logo=github)](https://codespaces.new/github/dotnet-codespaces)
[![Open in Dev Container](https://img.shields.io/static/v1?style=for-the-badge&label=Dev+Container&message=Open&color=blue&logo=visualstudiocode)](https://vscode.dev/redirect?url=vscode://ms-vscode-remote.remote-containers/cloneInVolume?url=https://github.com/github/dotnet-codespaces)

You can also run this repository locally by following these instructions: 
1. Clone the repo to your local machine `git clone https://github.com/github/dotnet-codespaces`
1. Open repo in VS Code

## Getting started

1. **📤 One-click setup**: [Open a new Codespace](https://codespaces.new/github/dotnet-codespaces), giving you a fully configured cloud developer environment.
2. **▶️ Run all, one-click again**: Use VS Code's built-in *Run* command and open the forwarded ports *8080* and *8081* in your browser. 

![Debug menu in VS Code showing Run All](images/RunAll.png)

3. The Blazor web app and Scalar can be open by heading to **/scalar** in your browser. On Scalar, head to the backend API and click "Test Request" to call and test the API. 

![A website showing weather](images/BlazorApp.png)

!["UI showing testing an API"](images/scalar.png)


4. **🔄 Iterate quickly:** Codespaces updates the server on each save, and VS Code's debugger lets you dig into the code execution.

5. To stop running, return to VS Code, and click Stop twice in the debug toolbar. 

![VS Code stop debuggin on both backend and frontend](images/StopRun.png)


## Contributing

This project welcomes contributions and suggestions.  Most contributions require you to agree to a
Contributor License Agreement (CLA) declaring that you have the right to, and actually do, grant us
the rights to use your contribution. For details, visit https://cla.opensource.microsoft.com.

When you submit a pull request, a CLA bot will automatically determine whether you need to provide
a CLA and decorate the PR appropriately (e.g., status check, comment). Simply follow the instructions
provided by the bot. You will only need to do this once across all repos using our CLA.

This project has adopted the [Microsoft Open Source Code of Conduct](https://opensource.microsoft.com/codeofconduct/).
For more information see the [Code of Conduct FAQ](https://opensource.microsoft.com/codeofconduct/faq/) or
contact [opencode@microsoft.com](mailto:opencode@microsoft.com) with any additional questions or comments.

## Trademarks

This project may contain trademarks or logos for projects, products, or services. Authorized use of Microsoft 
trademarks or logos is subject to and must follow 
[Microsoft's Trademark & Brand Guidelines](https://www.microsoft.com/en-us/legal/intellectualproperty/trademarks/usage/general).
Use of Microsoft trademarks or logos in modified versions of this project must not cause confusion or imply Microsoft sponsorship.
Any use of third-party trademarks or logos are subject to those third-party's policies.
