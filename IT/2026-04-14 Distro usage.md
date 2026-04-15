FleetDM werkt goed met oa:
- Ubuntu
- Mint

Andere disto users:
- Bruno - verschillende devices met verschillende distros -> Arch
- Bram - Debian
- Dominique, Nicolas, Robin - Arch linux
Dominique niet meer?

Open vraag: wat kan er niet op Ubuntu/Mint?

Tom - Mint

**Rutger en Jeroen**  BlueFin

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