Absolutely! Here's an example of how you can access secrets stored in Azure Key Vault within your .NET 8.0 application:

### Example Code

#### 1. Update `Program.cs`
First, ensure your `Program.cs` is set up to include Azure Key Vault configuration:

```csharp
using Azure.Identity;
using Azure.Extensions.AspNetCore.Configuration.Secrets;

var builder = WebApplication.CreateBuilder(args);

// Add Azure Key Vault
var keyVaultEndpoint = new Uri($"https://{builder.Configuration["KeyVaultName"]}.vault.azure.net/");
builder.Configuration.AddAzureKeyVault(keyVaultEndpoint, new DefaultAzureCredential());

var app = builder.Build();

// Other configurations...

app.Run();
```

#### 2. Access Secrets in Your Application
Next, you can access the secrets in your application code. For example, if you have a secret named `MySecret` in Azure Key Vault, you can retrieve it as follows:

```csharp
var secretValue = builder.Configuration["MySecret"];
Console.WriteLine($"Secret Value: {secretValue}");
```

### Full Example

Here's a more complete example that includes setting up a simple web application and accessing a secret:

```csharp
using Azure.Identity;
using Azure.Extensions.AspNetCore.Configuration.Secrets;

var builder = WebApplication.CreateBuilder(args);

// Add Azure Key Vault
var keyVaultEndpoint = new Uri($"https://{builder.Configuration["KeyVaultName"]}.vault.azure.net/");
builder.Configuration.AddAzureKeyVault(keyVaultEndpoint, new DefaultAzureCredential());

var app = builder.Build();

app.MapGet("/", (IConfiguration config) =>
{
    var secretValue = config["MySecret"];
    return Results.Ok($"Secret Value: {secretValue}");
});

app.Run();
```

### Steps Recap
1. **Initialize User-Secrets**:
   ```bash
   dotnet user-secrets init
   ```
2. **Add Key Vault Name to User-Secrets**:
   ```bash
   dotnet user-secrets set "KeyVaultName" "<YourKeyVaultName>"
   ```
3. **Add Secrets to Azure Key Vault**:
   ```bash
   az keyvault secret set --vault-name <YourKeyVaultName> --name MySecret --value "ThisIsASecretValue"
   ```

This example sets up a simple web application that retrieves a secret from Azure Key Vault and displays it.

Would you like more details on any specific part of this example?