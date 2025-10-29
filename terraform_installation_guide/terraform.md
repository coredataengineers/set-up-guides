# Terraform Installation Guide

This guide provides step-by-step instructions for installing Terraform on Windows, Linux, and macOS using package managers.

**Official Documentation:** [https://developer.hashicorp.com/terraform/tutorials/aws-get-started/install-cli](https://developer.hashicorp.com/terraform/tutorials/aws-get-started/install-cli)

---

## Table of Contents
- [Windows Installation](#windows-installation)
- [Linux Installation](#linux-installation)
- [macOS Installation](#macos-installation)
- [Verify Installation](#verify-installation)

---

## Windows Installation

### Using Chocolatey

1. **Install Chocolatey** (if not already installed)
   - Open PowerShell as Administrator
   - Run the following command:
   ```powershell
   Set-ExecutionPolicy Bypass -Scope Process -Force; [System.Net.ServicePointManager]::SecurityProtocol = [System.Net.ServicePointManager]::SecurityProtocol -bor 3072; iex ((New-Object System.Net.WebClient).DownloadString('https://chocolatey.org/install.ps1'))
   ```

2. **Install Terraform**
   ```powershell
   choco install terraform
   ```

3. **Restart your terminal** to apply the changes

---

## Linux Installation

### Using Package Manager (Ubuntu/Debian)

1. **Add HashiCorp GPG key**
   ```bash
   curl -fsSL https://apt.releases.hashicorp.com/gpg | sudo apt-key add -
   ```

2. **Add HashiCorp repository**
   ```bash
   sudo apt-add-repository "deb [arch=amd64] https://apt.releases.hashicorp.com $(lsb_release -cs) main"
   ```

3. **Update and install Terraform**
   ```bash
   sudo apt-get update && sudo apt-get install terraform
   ```

---

## macOS Installation

### Using Homebrew

1. **Install Homebrew** (if not already installed)
   ```bash
   /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
   ```

2. **Install Terraform**
   ```bash
   brew tap hashicorp/tap
   brew install hashicorp/tap/terraform
   ```

---

## Verify Installation

After installation on any platform, verify that Terraform is installed correctly:

```bash
terraform --version
```

or

```bash
terraform -v
```

You should see output similar to:
```
Terraform v1.9.2
on linux_amd64
```

---

## Next Steps

Once Terraform is installed, you can:

1. **Initialize a Terraform configuration**
   ```bash
   terraform init
   ```

2. **Create a Terraform configuration file** (`main.tf`)

3. **Plan your infrastructure changes**
   ```bash
   terraform plan
   ```

4. **Apply your configuration**
   ```bash
   terraform apply
   ```

---

## Updating Terraform

### Windows (Chocolatey)
```powershell
choco upgrade terraform
```

### Linux (Package Manager)
```bash
sudo apt-get update && sudo apt-get upgrade terraform
```

### macOS (Homebrew)
```bash
brew upgrade hashicorp/tap/terraform
```

---

## Additional Resources

- **Official Terraform Documentation:** [https://developer.hashicorp.com/terraform/docs](https://developer.hashicorp.com/terraform/docs)
- **Terraform Registry:** [https://registry.terraform.io/](https://registry.terraform.io/)
- **HashiCorp Learn:** [https://learn.hashicorp.com/terraform](https://learn.hashicorp.com/terraform)

---

## Troubleshooting

### Command not found error
If you receive a "command not found" error after installation:
- **Windows:** Restart your terminal or PowerShell
- **Linux/macOS:** Run `source ~/.bashrc` or `source ~/.zshrc`, or restart your terminal
- Verify the Terraform binary is in your PATH by running `echo $PATH` (Linux/macOS) or `echo %PATH%` (Windows)

### Permission denied error
- **Linux/macOS:** Ensure you have the necessary permissions to install packages with your package manager
- Run installation commands with `sudo` if necessary