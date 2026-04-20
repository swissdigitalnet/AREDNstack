# AREDN Firmware Installation Checklist

Step-by-step to-do list for flashing AREDN firmware on MikroTik HAP routers and SXTsq dishes.
Based on *AREDN Setup V2.3* by Andreas Spiess, HB9BLA.

> Differences for the **SXTsq dish** (single Ethernet port) are marked with: 📡

---

## Prerequisites

### 🔧 Hardware

- [ ] **MikroTik HAP AC Lite** or **HAP AC3** router
- [ ] 📡 *or* **MikroTik SXTsq** dish (single Ethernet port)
- [ ] **AC power supply** (24V) for the HAP router
- [ ] 📡 **PoE injector** (Y-cable) or passive PoE switch for the SXTsq (24V — the HAP power supply also works)
- [ ] **Dumb Ethernet switch** (unmanaged, no DHCP)
- [ ] **2x Ethernet cables** (one PC-to-switch, one switch-to-device)
- [ ] **SIP phone** (Yealink T-series recommended)
- [ ] **Ethernet cable** to connect the phone to the HAP router (port 2–4)
- [ ] **Paperclip or toothpick** for pressing the reset button

### 💻 Software & Files

- [ ] **Windows PC** with Ethernet port
- [ ] [AREDNstack repo](https://github.com/swissdigitalnet/AREDNstack) (download as ZIP)
- [ ] [Tiny PXE Server](http://erwan.labalec.fr/tinypxeserver/pxesrv.zip)
- [ ] **AREDN firmware** files for your device from https://downloads.arednmesh.org/afs/www/
  - **kernel** file (will be renamed to `rb.elf`)
  - **sysupgrade** file
- [ ] **Yealink phone firmware** (from `Firmware Repo/` in the AREDNstack repo, if needed)

---

## 1. Preparation

- [ ] Download the [AREDNstack repo](https://github.com/swissdigitalnet/AREDNstack) as ZIP and extract it
- [ ] Download the correct **kernel** and **sysupgrade** files for your device from https://downloads.arednmesh.org/afs/www/
  - HAP AC Lite: `mips-24kc` files
  - HAP AC3: `arm-cortex` files
  - 📡 SXTsq: check exact model name — every letter matters!
- [ ] Rename the kernel `.bin` file to **`rb.elf`**
- [ ] Download and unpack [Tiny PXE Server](http://erwan.labalec.fr/tinypxeserver/pxesrv.zip)
- [ ] Copy `rb.elf` into the PXE server's `files/` folder

---

## 2. Network Setup

- [ ] Disconnect your PC from Wi-Fi
- [ ] Set a fixed IP address on your PC:
  - IP: `192.168.1.50`
  - Subnet: `255.255.255.0`
  - Gateway: `192.168.1.1`
  - *(Windows: type `ncpa.cpl` in search → Ethernet → Properties → IPv4)*
- [ ] Connect your PC to a dumb switch
- [ ] Connect the target device to the same switch:
  - **HAP router**: use **port 1** (labeled "Internet")
  - 📡 **SXTsq**: use the **only Ethernet socket**
- [ ] Power on the switch

---

## 3. Power the Target Device

- [ ] **HAP router**: power with the **AC power supply** (do NOT use PoE)
- [ ] 📡 **SXTsq**: power with a **PoE injector** (Y-cable) or passive PoE switch (24V — the HAP power supply also works)
- [ ] Wait until the device is ready:
  - **HAP**: wait until the top red LED is off and the green LED #1 flickers (~3 minutes)
  - 📡 **SXTsq**: wait for power LED to stabilize

---

## 4. Flash rb.elf via PXE Boot

- [ ] Start `pxesrv.exe` (allow it through Windows firewall if prompted)
- [ ] Select IP `192.168.1.50` from the dropdown
- [ ] Select `rb.elf` as the Boot File
- [ ] Uncheck "Filename if user class..."
- [ ] **Unplug power** from the target device
- [ ] Click **"Online"** in Tiny PXE Server
- [ ] Press and **hold the reset button** on the target device (use a paperclip)
- [ ] **Plug power back in** while holding reset
- [ ] Watch the PXE log — when you see `"Do ReadFile:rb.elf ..."` **release the reset button** (~20 seconds)
- [ ] Click **"Offline"** in Tiny PXE Server immediately after
- [ ] **Do NOT unplug power** — keep the device powered!
- [ ] Wait a few minutes for the process to complete

> If new requests keep appearing in the PXE window, try a different PC (one with minimal software).

---

## 5. Flash AREDN Sysupgrade Firmware

- [ ] Move the Ethernet cable:
  - **HAP router**: move cable from port 1 to **port 2**
  - 📡 **SXTsq**: leave cable in the **same (only) socket**
- [ ] Open a browser and go to `http://192.168.1.1`
- [ ] Verify you see the AREDN initial setup page (if not, go back to step 4)
- [ ] Select the **sysupgrade file** for your device and click **"Upload"**
- [ ] Wait until `http://192.168.1.1` responds again
- [ ] Enter your **node name** (must begin with your callsign)

---

## 6. Configure AREDN

- [ ] Change your PC back to **DHCP**
- [ ] Open `http://localnode.local.mesh` in your browser
- [ ] If no response, wait and retry (can take up to 15 minutes)
- [ ] Log in to the administration interface
- [ ] Configure radio settings:
  - Recommended: Wi-Fi on, 5 GHz mesh enabled
  - If connected via cable to internet: 2.4 GHz radio can be turned "off"
- [ ] **Commit** changes when prompted
- [ ] Reboot if asked

---

## 7. Make Your Phone Visible on the Network

- [ ] Connect your SIP phone to HAP router **port 2–4**
- [ ] Go to the **LAN DHCP** area on the AREDN admin page
- [ ] Find your phone in the list and press **"+"**
- [ ] Change its name to your assigned phone number
- [ ] **Untick** "do not propagate" so the phone is visible on the mesh
- [ ] Verify: go to **Node Status → Mesh Status** — phone should appear under your router

---

## 8. Install the Phonebook

- [ ] Download the correct `.ipk` file from https://github.com/arednch/packages/releases
  - HAP AC Lite: `mips-24kc`
  - HAP AC3: `arm-cortex`
- [ ] Go to **Installed Packages** in the AREDN admin UI
- [ ] Upload the `.ipk` file to install
- [ ] Reboot the router
- [ ] Verify phonebook: open `http://localnode.local.mesh:8081/phonebook?format=direct&target=generic&ia=true`
- [ ] If empty, force reload: `http://localnode.local.mesh:8081/reload`
