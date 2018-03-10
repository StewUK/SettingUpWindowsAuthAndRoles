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
