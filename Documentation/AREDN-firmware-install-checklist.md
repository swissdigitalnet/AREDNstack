# AREDN Firmware Installation Checklist

Step-by-step to-do list for flashing AREDN firmware on MikroTik HAP routers and SXTsq dishes.
Based on *AREDN Setup V2.3*.

> Differences for the **SXTsq dish** (single Ethernet port) are marked with: 📡

---

## Prerequisites

### 🔧 Hardware

- [ ] **MikroTik HAP AC Lite** or **HAP AC3** router
- [ ] 📡 *or* **MikroTik SXTsq** dish (single Ethernet port)
- [ ] **AC power supply** (12V or 24V) for the HAP router
- [ ] 📡 **PoE injector** (Y-cable) or passive PoE switch for the SXTsq (12V or 24V)
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

## 3. Flash rb.elf via PXE Boot

> Leave the target device **unpowered** until the PXE server is Online.

- [ ] Start `pxesrv.exe` (allow it through Windows firewall if prompted)
- [ ] Select IP `192.168.1.50` from the dropdown
- [ ] Select `rb.elf` as the Boot File
- [ ] Uncheck "Filename if user class..."
- [ ] Click **"Online"** in Tiny PXE Server
- [ ] Press and **hold the reset button** on the target device (use a paperclip)
- [ ] **Plug power in** (HAP: AC adapter — do NOT use PoE; 📡 SXTsq: PoE injector / passive PoE switch) while holding reset
- [ ] Front **green LED** cycles on → flashing → off (~5 s each) while booting into PXE
- [ ] Watch the PXE log — when you see `"Do ReadFile:rb.elf ..."` **release the reset button** (~20 seconds total)
- [ ] Click **"Offline"** in Tiny PXE Server immediately after
- [ ] **Do NOT unplug power** — keep the device powered!
- [ ] Wait a few minutes for the process to complete

> If new requests keep appearing in the PXE window, try a different PC (one with minimal software).
> Don't hold reset too long or you'll have to start over.

---

## 4. Flash AREDN Sysupgrade Firmware

- [ ] Move the Ethernet cable:
  - **HAP router**: move cable from port 1 to **port 2**
  - 📡 **SXTsq**: leave cable in the **same (only) socket**
- [ ] Open a browser and go to `http://192.168.1.1`
- [ ] Verify you see the AREDN initial setup page (if not, go back to step 3)
- [ ] Select the **sysupgrade file** for your device and click **"Upload"**
- [ ] Wait until `http://192.168.1.1` responds again (~3 min — do not power-cycle)
- [ ] The firstuse form loads. Enter:
  - **Node name** — must begin with your callsign in capitals (e.g. `HB9BLA-hap-2`)
  - **Password** — ≥4 chars, no `# ' "`, enter in both boxes
- [ ] Click save — device reboots once more (~1–2 min)

---

## 5. Configure AREDN

> After firstuse, AREDN **moves the LAN off `192.168.1.x`** onto a random `10.x.x.x/29` range. Pinging `192.168.1.1` will fail — that's expected, not a boot failure.

- [ ] Change your PC back to **DHCP**
- [ ] Force a fresh DHCP lease so the PC picks up the new subnet: `ipconfig /release && ipconfig /renew` in a command prompt. (Without this, the PC may keep its old `192.168.1.20` lease until it expires.)
- [ ] Check the new LAN addressing: `ipconfig` → your Ethernet adapter should have an IPv4 like `10.x.x.x` with DNS suffix `local.mesh`. The **Default Gateway** is the hAP's new LAN IP.
- [ ] Open **`http://localnode.local.mesh/`** — this is the normal way to reach an AREDN node. `http://localnode/` (short name) and the gateway IP also work as fallbacks.
- [ ] Log in to the administration interface (user `root`, password you just set)
- [ ] Configure radio settings:
  - Recommended: Wi-Fi on, 5 GHz mesh enabled
  - If connected via cable to internet: 2.4 GHz radio can be turned "off"
- [ ] **Commit** changes when prompted
- [ ] Reboot if asked

---

## 6. Install the Phonebook

- [ ] Download the correct `.ipk` file from https://github.com/swissdigitalnet/AREDN-Phonebook/releases
  - HAP AC Lite (ath79): `mips_24kc` file
  - HAP AC3 (ipq40xx): `arm_cortex-a7_neon-vfpv4` file
- [ ] In the AREDN admin UI go to **Administration → Package Management**, upload the `.ipk`, click **Fetch and Install**
- [ ] The `AREDN-Phonebook` service starts automatically and populates the XML within seconds — **no reboot required** on fresh install (after an upgrade a power-cycle can help)
- [ ] Verify: open `http://localnode.local.mesh/arednstack/phonebook_generic_direct.xml` — the Yealink directory XML should load (~25 KB)
- [ ] (Optional) Open the AREDNmon dashboard: `http://localnode.local.mesh/cgi-bin/arednmon`

---

## 7. Make Your Phone Visible on the Network

- [ ] Connect your SIP phone to HAP router **port 2–4**
- [ ] Go to the **LAN DHCP** area on the AREDN admin page
- [ ] Find your phone in the list and press **"+"**
- [ ] Change its name to your assigned phone number
- [ ] **Untick** "do not propagate" so the phone is visible on the mesh
- [ ] Verify: go to **Node Status → Mesh Status** — phone should appear under your router
