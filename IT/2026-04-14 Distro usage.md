FleetDM werkt goed met oa:
- Ubuntu
- Mint

Andere disto users:
- Bruno - verschillende devices met verschillende distros -> Arch
- Bram - Debian
-
- Dominique, Nicolas, Robin - Arch linux
Dominique niet meer?

Open vraag: wat kan er niet op Ubuntu/Mint?

Tom - Mint

**Rutger en Jeroen**  BlueFin


**Status**

Robin
- draait arch, heeft problemen gehad in het verleden met Ubuntu, personal preference. Nu op ASML project maakt minder uit door VDI
- draait fleetdm (zie instructies), problemen gehad met versie incompat. Wazuh draaide ooit niet (aangesproken door Martin), assumptie = nu in orde

Lander
- draait nu windows (VDI werkt beter) - als distro dan liefst Fedora

Rutger
- 

Nicolas
- 

Bruno
- 

Bram
- 




**Getting FleetDM to work on Arch linux**

The core reason the FleetDM agent (`fleetd`/`orbit`) behaves differently on Arch Linux compared to Ubuntu is that **Arch Linux is not officially supported** by Fleet's default installer.

Ubuntu (and Debian/RHEL) users typically use the pre-compiled `.deb` or `.rpm` packages which include `osquery` binaries pre-configured for those package managers. On Arch, several technical "disconnects" cause the agent to fail or report missing data.

### 1. The Package Manager Conflict

By default, Fleet's software inventory queries look for `deb_packages` or `rpm_packages`. Since Arch uses `pacman`, the standard Fleet queries often return empty results or errors because the `osquery` binary bundled with the agent isn't looking at the Arch package database.

**The Fix:**

You need to ensure `osquery` is querying the `pacman_packages` table.

- **AUR Package:** Use the community-maintained `fleet-desktop` or `fleet-orbit` packages in the AUR. These are often patched to bridge these gaps.
    
- **Custom Queries:** In your Fleet console, you may need to add a specific query for your Arch hosts:
    
    SQL
    
    ```
    SELECT name, version, 'pacman' AS package_manager FROM pacman_packages;
    ```
    

---

### 2. Dependency & Path Issues

Fleet's `orbit` (the updater/wrapper) expects certain filesystem paths that follow the Filesystem Hierarchy Standard (FHS) as implemented in Ubuntu. Arch sometimes places binaries or configuration files in different locations (e.g., `/usr/bin` vs `/bin` symlinks).

**How to make it work properly:**

1. **Manual Binary Installation:** Instead of using the Ubuntu `.deb` installer (which some users try to "force" onto Arch), use the `fleetctl` tool to generate a **Linux Tarball** (`.tar.gz`) instead of a package.
    
    Bash
    
    ```
    fleetctl package --export linux --os linux
    ```
    
2. **Systemd Service:** Arch is strict about `systemd` units. If you are manually installing, ensure your service file points to the correct absolute paths. The standard Ubuntu service might look for `/usr/bin/orbit`, but if you’ve placed it in `/usr/local/bin`, it will fail silently on boot.
    

---

### 3. Missing `osquery` Tables

On Arch, if you install `osquery` via `pacman -S osquery` instead of using the version bundled with the Fleet agent, you might find that certain tables (like `disk_encryption`) are disabled by the Arch packagers due to library dependencies.

**The Fix:**

Always use the version of `osqueryd` provided by the Fleet Orbit agent rather than the system-wide Arch package. This ensures the agent has the specific flags and features Fleet expects.

---

### Summary Checklist for Arch

- **Don't use `debtap`:** Do not try to convert the Ubuntu `.deb` to an Arch package. It leads to broken paths.
    
- **Use the AUR:** Check [AUR (fleet-desktop)](https://aur.archlinux.org/packages/fleet-desktop) for a version specifically built for Arch’s environment.
    
- **Check glibc compatibility:** If you are using an older Arch install, ensure your `glibc` is up to date, as the `orbit` binary is compiled against modern C libraries.
    
- **Log Inspection:** If the agent still won't check in, run this to see exactly where it's tripping:
    
    Bash
    
    ```
    sudo journalctl -u orbit -f
    ```


---
Input Robin

## Fleet
Debtap can be used to install fleet from the deb package. You can generate your own deb or use the one created by IT.
1) Install debtap from the aur ([https://aur.archlinux.org/packages/debtap](https://aur.archlinux.org/packages/debtap))
2) Initialize debtap: `sudo debtap -u`
3) Optional: Generate fleet deb
```bash
# Install fleetctl from [https://github.com/fleetdm/fleet/releases](https://github.com/fleetdm/fleet/releases)
fleetctl package --type=deb --fleet-url=[https://fleetdm.klarrio.com](https://fleetdm.klarrio.com) --enroll-secret=<YOUR_SECRET> --fleet-certificate=PATH_TO_YOUR_CERTIFICATE/fleet.pem
```
4) `debtap <path-to-deb>`
5) `sudo pacman -U <generated-package.zst>`
6) `sudo cp /etc/default/orbit /etc/orbit`
7) `sudo systemctl start orbit.service`
8) `sudo systemctl enable orbit.service`

## Wazuh
There is no Archlinux package available for Wazuh, and debtap fails. It can however be installed from source quite easily.
1) Install dependencies, see [https://github.com/wazuh/wazuh/blob/master/INSTALL](https://github.com/wazuh/wazuh/blob/master/INSTALL). I'm not 100% sure if it's necessary, but the following package is listed as a requirement: [https://aur.archlinux.org/packages/policycoreutils](https://aur.archlinux.org/packages/policycoreutils).
2) Clone the git repo [https://github.com/wazuh/wazuh](https://github.com/wazuh/wazuh)
3) Checkout the right version (4.4.2)
4) Run the installer (`sudo ./install.sh`) and follow the prompts. When asked about the type, select "agent". You can skip the certificate when prompted for it. It should ask for a URL, fill in `wazuh.klarrio.com`. For other prompts, just say yes.
5) Store the password in the designated file and fix the permissions. See [https://documentation.wazuh.com/current/user-manual/agent-enrollment/security-options/using-password-authentication.html#linux-unix-endpoint](https://documentation.wazuh.com/current/user-manual/agent-enrollment/security-options/using-password-authentication.html#linux-unix-endpoint) - IT should know the password.
6) `sudo systemctl start wazuh-agent`
7) `sudo systemctl enable wazuh-agent`

## DriveStrike
Drivestrike has an installer for Archlinux. Installation should be straightforward. Don't forget to start and enable the systemd service.
[https://app.drivestrike.com/instructions/linux/#Arch](https://app.drivestrike.com/instructions/linux/#Arch)

## ClamAV
ClamAV has a native package and good documentation. Install it and start/enable both the clamav-freshclam.service and clamav-daemon.service.
[https://wiki.archlinux.org/title/ClamAV](https://wiki.archlinux.org/title/ClamAV)
Note: for some reason freshclam broke on my laptop after rebooting, so check make sure it keeps running at first.