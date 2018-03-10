# SettingUpWindowsAuthAndRoles

-- Edit Program.cs (CreateWebHostBuilder method)

```
            WebHost.CreateDefaultBuilder(args)
                .UseHttpSys(options =>  
                {  
                    options.Authentication.Schemes =   
                        AuthenticationSchemes.NTLM | AuthenticationSchemes.Negotiate;  
                    options.Authentication.AllowAnonymous = false;  
                })  
                .UseStartup<Startup>();
```

-- Edit Startup.cs (ConfigureServices method)

```
            services.AddAuthentication(IISDefaults.AuthenticationScheme);  
            services.AddAuthentication(HttpSysDefaults.AuthenticationScheme);  

            services.AddAuthorization(options =>
            {
                options.AddPolicy("RequireANPRAdministratorsRole", policy => policy.RequireRole("ANPRAdministrators"));
                options.AddPolicy("RequireANPRUsersRole", policy => policy.RequireRole("ANPRUsers"));
                options.AddPolicy("RequireAdministratorsRole", policy => policy.RequireRole("Administrators"));
            });
```

-- Edit Controller to add Authorize (depending on role/policy)
https://docs.microsoft.com/en-us/aspnet/core/security/authorization/views?tabs=aspnetcore2x
```
[Authorize(Policy = "RequireANPRAdministratorsRole")]
```

-- Web page changes (top of the page)
```
@using Microsoft.AspNetCore.Authorization
@inject IAuthorizationService AuthorizationService
```

-- Web page changes (where required)
```
@if ((await AuthorizationService.AuthorizeAsync(User, "PolicyName")).Succeeded)
{
    <p>This paragraph is displayed because you fulfilled PolicyName.</p>
}
```
