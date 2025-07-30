# NuGet Test Project

A minimal .NET 8 application demonstrating NuGet package management with dependency lock files for security scanning
with tools like Trivy.

## Adding New Dependencies

### Method 1: Using dotnet CLI (Recommended)

```bash
# Add a new package
dotnet add package <PackageName> --version <Version>

# Restore and regenerate lock file
dotnet restore
```

### Method 2: Manual .csproj Edit

1. Edit the `.csproj` file and add the new package reference:
   ```xml
   <PackageReference Include="NewPackage" Version="1.0.0" />
   ```

2. Restore packages to regenerate the lock file:
   ```bash
   dotnet restore
   ```

### Important Notes

- **Always commit `packages.lock.json`** after adding/updating dependencies
- The lock file must be regenerated whenever dependencies change
- Use specific version numbers for reproducible builds

## Building and Running

```bash
# Restore packages (if needed)
dotnet restore

# Build the project
dotnet build

# Run the application
dotnet run
```

## Security Scanning

This project is configured for security scanning with tools like Trivy:

```bash
# Scan for vulnerabilities
trivy fs --scanners vuln .

# Generate detailed report
trivy fs --scanners vuln --format json --output security-report.json .

# Generate CycloneDX SBOM file
trivy fs --scanners vuln --format cyclonedx --output sbom.json .
```

The `packages.lock.json` file allows security scanners to detect all dependencies without requiring a build or package
restore operation.

## Troubleshooting

### Package restore issues

- Clear NuGet cache: `dotnet nuget locals all --clear`
- Delete `obj/` and `bin/` folders, then run `dotnet restore`