# Comprehensive Windows 11 WSL 2, Ubuntu & Docker Environment Setup Manual

This comprehensive guide covers the step-by-step installation, user configuration, environment optimization, Docker environment options (Docker Desktop vs. Native Docker Engine), and deep-dive troubleshooting for the Windows Subsystem for Linux (WSL 2) running Ubuntu on Windows 11.

---

## Table of Contents
1. [Part 1: WSL 2 & Ubuntu Clean Installation](#part-1-wsl-2--ubuntu-clean-installation)
2. [Part 2: First-Time Launch & User Initialization](#part-2-first-time-launch--user-initialization)
3. [Part 3: Docker Implementation - Option A: Docker Desktop (Windows Hybrid)](#part-3-docker-implementation---option-a-docker-desktop-windows-hybrid)
4. [Part 4: Docker Implementation - Option B: Native Docker Engine (Inside Ubuntu Terminal)](#part-4-docker-implementation---option-b-native-docker-engine-inside-ubuntu-terminal)
5. [Part 5: Comprehensive Troubleshooting & Error Resolution](#part-5-comprehensive-troubleshooting--error-resolution)

---

## Part 1: WSL 2 & Ubuntu Clean Installation

Setting up WSL 2 on Windows 11 is handled directly through an administrative command line interface. Follow these steps to ensure all system hypervisor architectures are initialized correctly:

1. **Open Terminal as Administrator:**
   - Right-click the Windows Start menu button and select **Terminal (Admin)** or **PowerShell (Admin)**.

2. **Execute the Automated Installation Command:**
   - Enter the following command and press `Enter`:
     ```powershell
     wsl --install -d Ubuntu
     ```
   - This single pipeline activates the mandatory Virtual Machine Platform, fetches the latest stable Linux kernel binary package from Microsoft, and downloads the default Ubuntu Long-Term Support (LTS) distribution image.

3. **Reboot the Host OS:**
   - Once the terminal window confirms installation completion, **restart your Windows 11 PC** to apply the virtualized platform changes at the hardware kernel level.

---

## Part 2: First-Time Launch & User Initialization

After your computer boots back up into Windows 11, you must perform an initial health-check verification sequence before creating your primary user environment.

### 1. Launching the App Environment
- Press the **Windows Key** on your keyboard, type **Ubuntu** into the search text field, and click the orange application icon.
- A terminal window will open up showing an initialization state: `Installing, this may take a few minutes...`

### 2. The Verification Checkpoint
- **Scenario A (Failure):** If the window suddenly crashes or outputs an error string (e.g., `WslRegisterDistribution failed with error: 0x80370102`), skip directly to **Part 5: Troubleshooting**.
- **Scenario B (Success):** If the installation completes without failure flags, the window will switch prompts and output: `Enter new UNIX username:`. You are safe to continue.

### 3. Setting Up Your Linux Credentials
1. **Enter Username:** Type a clean, lowercase name without spaces (e.g., `devuser`) and press `Enter`.
2. **Enter Password:** Input a secure password and press `Enter`.
3. **Retype Password:** Re-enter the identical password to verify accuracy.

> ⚠️ **CRITICAL SECURITY NOTE:** When entering passwords inside Linux terminals, **the display will remain completely blank**. No characters, placeholders, dots, or asterisks will show up. This hidden input is expected security behavior. Type your password blindly and hit `Enter`.

### 💡 The Importance of Your UNIX Password
The password you choose here does **not** link to your Microsoft or Windows login account. It is exclusively for this isolated Linux container environment. 
* **When is it used?** You will need this password anytime you prefix a terminal execution with `sudo` (SuperUser Do) to install development binaries, alter configuration arrays, or execute administrative packages (`sudo apt upgrade`).
* **Warning:** If you lose this password, you will be locked out of root execution blocks, requiring a recovery procedure through Windows PowerShell via complex container configuration mapping overrides.

---

## Part 3: Docker Implementation - Option A: Docker Desktop (Windows Hybrid)

This is the standard approach for developer setups that want a graphic user interface (GUI) console on Windows while mapping container lifecycles seamlessly into the Ubuntu instance.

### Step 1: Install Docker Desktop on Windows
1. Open your Windows browser and navigate to the official landing site: [https://www.docker.com/products/docker-desktop/](https://www.docker.com/products/docker-desktop/).
2. Download the **Docker Desktop for Windows** executable installer.
3. Launch the downloaded `.exe` application file.
4. **Crucial Setup Verification:** On the configuration splash screen, make sure the checkbox labeled **"Use WSL 2 instead of Hyper-V (recommended)"** is fully checked.
5. Let the setup script complete and click **Close and Restart** if prompted.

### Step 2: Configure WSL 2 Integration & Fix Missing Commands
If your Ubuntu distribution reports that `docker` or `docker-compose` can't be resolved, you must turn on the backend bridging network:
1. Open the **Docker Desktop** application from your Windows Start Menu.
2. Click the **Gear Icon (Settings)** located at the top right toolbar area.
3. Inside the **General** navigation menu panel on the left, verify that **"Use the WSL 2 based engine"** remains enabled.
4. Switch to the **Resources** option in the sidebar, and select **WSL Integration**.
5. Locate the toggle switch labeled **Ubuntu** and click it to **ON**.
6. Click the blue **Apply & restart** button in the lower-right workspace layout interface.
7. Open a **fresh Ubuntu Terminal window** and execute a validation verification parameter check:
   ```bash
   docker compose version
   ```

---

## Part 4: Docker Implementation - Option B: Native Docker Engine (Inside Ubuntu Terminal)

[Install Docker Engine in Ubuntu - Official documentation](https://docs.docker.com/engine/install/ubuntu/)

*Note: Use this method if you do not want to run Docker Desktop on Windows (due to corporate licensing limits or memory overhead), and instead prefer a pure, native Linux Docker environment running completely inside the terminal.*

### Step 1: Clean House & Add Official Repository Keys
Open your Ubuntu terminal and uninstall any conflicting legacy distribution artifacts:
```bash
sudo apt-get remove docker docker-engine docker.io containerd runc
```
Update your local tracking matrix, add security certificates, and seed the verified GPG key configuration structures:
```bash
# Add Docker's official GPG key:
sudo apt update
sudo apt install ca-certificates curl
sudo install -m 0755 -d /etc/apt/keyrings
sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc
```

Expose the package repository paths inside Ubuntu's Advanced Package Tool (`apt`) software index files:
```bash
sudo tee /etc/apt/sources.list.d/docker.sources <<EOF
Types: deb
URIs: https://download.docker.com/linux/ubuntu
Suites: $(. /etc/os-release && echo "${UBUNTU_CODENAME:-$VERSION_CODENAME}")
Components: stable
Architectures: $(dpkg --print-architecture)
Signed-By: /etc/apt/keyrings/docker.asc
EOF

sudo apt update
```

### Step 2: Install Docker Engine & Compose Plugins
Synchronize your system index maps and download the live application runtimes directly from Docker's deployment stream:
```bash
sudo apt-get update
sudo apt install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

### Step 3: Configure WSL 2 Systemd Management
WSL 2 doesn't manage background services natively out-of-the-box unless system configuration parameters are mapped. Let's make sure the Docker background daemon starts up automatically whenever you load your environment.
1. Open the WSL configuration environment initialization controller file:
   ```bash
   sudo nano /etc/wsl.conf
   ```
2. Paste the following structured blocks exactly as written:
   ```ini
   [boot]
   systemd=true
   ```
3. Press `Ctrl + O` then `Enter` to write out the data stream matrix, then `Ctrl + X` to exit the nano interface window.
4. Open a **Windows PowerShell (Admin)** console and run a hard shutdown on the active hypervisor instances to force an internal config refresh:
   ```powershell
   wsl --shutdown
   ```
5. Relaunch your Ubuntu terminal application window and check your native engine state properties:
   ```bash
   docker compose version
   docker ps
   ```

---

## Part 5: Comprehensive Troubleshooting & Error Resolution

### 🔴 Problem 1: `WslRegisterDistribution failed with error: 0x80370102`
* **Root Cause:** Your system's central processing hardware layer is actively blocking virtualization parameters, or the underlying Windows virtualization container engine components have not been checked.

#### Step A: Verify Physical System State with Windows Task Manager
1. Hit `Ctrl + Shift + Esc` to run the **Task Manager**.
2. Click the **Performance** navigation icon block and click **CPU**.
3. Locate the status line fields listed on the lower right dashboard. Inspect the **Virtualization** readout:
   - **Enabled:** Proceed to **Step C (Windows Configuration)**.
   - **Disabled:** Your computer's physical motherboard firmware is suppressing virtual computing. Proceed to **Step B (BIOS)**.

#### Step B: Enable Hardware Virtualization inside Motherboard BIOS
1. Restart your PC. The moment your monitor signals go black and cycle back on, continuously strike your physical BIOS layout command key (typically **Del**, **F2**, or **F12** depending on your motherboard brand like ASUS, MSI, Gigabyte, Lenovo, Dell, or HP).
2. Look for an **Advanced CPU Configuration**, **Processor Parameters**, or **Overclocking/Tweaking** management layer panel workspace (you may need to strike **F7** to enter Advanced Mode options).
3. Search for the processor virtualization flag controls matching your hardware model line architecture:
   - **Intel Processors:** Locate **Intel Virtualization Technology**, **VT-x**, or **Vanderpool** $
ightarrow$ toggle value to **Enabled**.
   - **AMD Processors:** Locate **SVM Mode**, **Secure Virtual Machine**, or **AMD-V** $
ightarrow$ toggle value to **Enabled**.
4. Press **F10** on your physical keyboard to lock structural system updates and restart the computer automatically back into the Windows 11 Desktop environment.

#### Step C: Force-Enable Optional Core Framework Components
1. Press your Windows Key, search for **Turn Windows features on or off**, and launch the legacy control window dialog component tool module.
2. Track down the options matrix table stack and verify that **both** target features are completely populated with checkmarks:
   - `[✓] Virtual Machine Platform`
   - `[✓] Windows Subsystem for Linux`
3. Hit **OK**, wait for the Windows Update component extraction pipelines to inject the platform libraries, and select **Restart Now** when prompted.

---

### 🔴 Problem 2: `The command 'docker-compose' could not be found...`
* **Root Cause:** The Docker pipeline hooks are either missing configuration references, or Docker Desktop hasn't injected its linking infrastructure files inside your Ubuntu container subsystem profile paths.

#### Solution Sequence:
1. Ensure **Docker Desktop** is open and active on your Windows workspace. It cannot bridge connections if the application process isn't running in the Windows background tray.
2. Follow the steps detailed in **Part 3, Step 2** to toggled the Ubuntu container bridge framework profile switch **ON**.
3. Keep in mind that modern layouts prioritize space-delimited options (`docker compose`) over legacy hyphenated scripts (`docker-compose`). Verify runtime properties explicitly by running:
   ```bash
   docker compose version
   ```
