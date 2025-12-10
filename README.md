## Download RDCMan (Remote Desktop Connection Manager)

Download the latest release from Releases:       
https://github.com/ntfsid/RDCMan/releases/tag/v3.1

## Introduction

RDCMan facilitates the management of multiple remote desktop connections. It proves invaluable when handling server labs that require frequent access to each machine, such as automated check-in systems or data centers.

Servers are arranged into named groups. With a single command, you can connect or disconnect all servers in a group. All servers in a group can be viewed as thumbnails, displaying live activity for each session. Servers may inherit logon credentials from a parent group or a credential store. Consequently, changing your lab account password requires updating it only once in RDCMan. Passwords are securely stored using either CryptProtectData with the logged-on user's authority or via an X509 certificate.

Users on OS versions prior to Win7/Vista will need Terminal Services Client version 6, available from the Microsoft Download Center: XP; Win2003.

Upgrade note: RDG files created with this RDCMan version are incompatible with older versions. Any legacy RDG file opened and saved will be backed up as filename.old.

## The Display

The RDCMan interface consists of a menu, a server tree, a splitter bar, and a client area.

### The Menu

RDCMan provides several primary menus:

* **File** – load, save, or close RDCMan file groups
* **Edit** – add, remove, or modify server and group properties
* **Session** – connect, disconnect, or log off sessions
* **View** – control server tree visibility, virtual groups, and client area size
* **Remote Desktops** – navigate groups and servers hierarchically, useful when the Server Tree is hidden
* **Tools** – adjust application settings
* **Help** – access RDCMan documentation

### The Tree

Tasks such as adding, removing, or editing servers and groups are typically done via right-clicking a tree node. Drag-and-drop allows repositioning servers and groups.

Keyboard shortcuts:

* **Enter**: Connect to the selected server
* **Shift+Enter**: Connect using the Connect As feature
* **Delete**: Remove selected server or group
* **Shift+Delete**: Remove without confirmation
* **Alt+Enter**: Open properties for the selected server or group
* **Tab**: Focus on a connected server

The **\[View\.Server tree location]** menu option sets the tree to the left or right side.
The server tree can be docked, auto-hidden, or always hidden via **\[View\.Server tree visibility]**. Even when hidden, servers remain accessible via the Remote Desktops menu. With auto-hide, the splitter bar stays visible on the left; hovering over it restores the tree.

### The Client Area

The client area reflects the selected node. Selecting a server shows its remote desktop client; selecting a group shows thumbnails of its servers. The client area size can be adjusted via the View menu or by resizing the RDCMan window. Use **\[View\.Lock window size]** to prevent resizing.

**Caution:** Keyboard navigation may give focus to a connected server in the thumbnail view. Use **\[Display Settings.Allow thumbnail session interaction]** to control this behavior.

### Full Screen Mode

To enter full screen, select a server and press **Ctrl+Alt+Break** (configurable). Exit using the same shortcut or the minimize/restore buttons. Multiple monitors can be spanned if enabled.

### Shortcut Keys

A full list of Terminal Services shortcuts is available, with some configurable in the Hot Keys tab.

## Files

RDCMan organizes connections in file groups, which are collections of groups and/or servers stored in a single file. Servers cannot exist outside groups, and groups cannot exist outside files.

## Groups

Groups hold server lists and configuration settings, including logon credentials. Settings may inherit from parent groups or defaults. Groups can be nested but are homogeneous: they contain either groups or servers, not both. Servers within a group can be connected or disconnected simultaneously.

When selected, groups display their servers as thumbnails. Thumbnails may show live server windows or connection status. Global thumbnail settings are in **\[Tools.Options.Client Area]**; group-specific settings are in Display Settings.

### Smart Groups

Smart groups populate dynamically based on rules. Ancestors of sibling groups may be included.

### The Connected Virtual Group

Connected servers are automatically added here. They cannot be manually added or removed. Toggle via the View menu.

### The Reconnect Virtual Group

Servers that disconnect temporarily (e.g., after OS updates) can be dragged here. RDCMan will attempt reconnection until successful. Toggle via the View menu.

### The Favorites Virtual Group

A flat list of favorite servers, useful when frequently accessing a subset of servers. Toggle via the View menu.

### The Connect To Virtual Group

Holds servers not in user-created groups. Visible only when ad hoc connections exist.

### The Recent Virtual Group

Lists recently accessed servers. Toggle via the View menu.

## Servers

Each server has a network name or IP, optional display name, and logon information (which may be inherited).

### Adding Servers Manually

Servers following patterns can be bulk-added:

* Iteration: `{a,b,c}`
* Range: `[1-5]` (prefix 0s for width)

Examples:

* `server1{a,b,c}` → `server1a`, `server1b`, `server1c`
* `server[001-15]` → `server001` … `server015`
* `{dca,dcb}rack[1-5]sql[1-2]` → multiple combinations

### Importing Servers from a Text File

Use one server name per line:

```txt
Server1
SecondServer
YANS
```

Duplicate server names update existing preferences.

### Ad Hoc Connections

Create via **\[Session.Connect to]**. Added servers go to Connect To Virtual Group; moving them to user-created groups makes them permanent. Remaining servers are discarded on exit.

### Windows Azure

In **\[Connection Settings]**, enter role name and instance into *Load balance config*, e.g.:

```
Cookie: mstshash=MyServiceWebRole#MyServiceWebRole_IN_0#Microsoft.WindowsAzure.Plugins.RemoteAccess.Rdp
```

### Session Actions

Focus can switch between sessions or the server tree:

* **Ctrl+Alt+Left**: select previous session
* **Ctrl+Alt+Right**: choose session or server tree

Certain actions like **Ctrl+Alt+Del** require **\[Session.Send keys]** or **\[Session.Remote actions]**.

## Global Options

Access via **\[Tool.Options]**. Modify client area size globally; most server-specific options apply on next connection.

### General

* *Hide main menu until ALT pressed*
* *Auto save interval*: define interval in minutes; 0 disables periodic save
* *Prompt to reconnect servers*: toggle reconnect prompts on startup
* *Default group settings*: base inheritance settings

### Tree

* *Click to select gives focus to remote client*
* *Dim inactive nodes*: visually distinguish keyboard focus

### Client Area

* *Client Area Size*: resize panel; also via **\[View\.Client size]**
* *Thumbnail Unit Size*: absolute pixels or percentage

### Full Screen

* *Show full screen connection bar / Auto-hide*
* *Always on top / Multi-monitor support*

### Local Options

Groups and servers have tabbed property pages. *Inherit from parent* applies parent settings. Most changes apply on next connection.

### File, Group, Server Settings

Specify names, nesting, and comments. Connect SCVMM virtual machines via PowerShell:

```powershell
get-vm | ft ElementName,Name,Id
```

### Logon Credentials

Set username, password, domain. Use `domain\user` format; `[server]` or `[display]` placeholders supported.

### Gateway Settings

Options for TS Gateway: server name, authentication, local bypass. Vista SP1+ adds explicit credentials and sharing.

### Connection Settings

Customize session connection, console access, ports, and post-connection programs. Programs run only on first console connection.

### Remote Desktop Settings

Logical desktop size vs. client view. Scroll bars used if client smaller. Options: *Same as client area* or *Full screen*. Maximums: Version 5: 1600x1200, Version 6: 4096x2048.

### Local Resources

Redirect sound, Windows key combos, drives, ports, printers, smart cards, and clipboard as needed.

### Security Settings

Require authentication before connection.

### Display Settings

Thumbnail scale, preview session, interaction, show disconnected. Scale >1 enlarges important servers.

### Encryption Settings

Encrypt passwords via CryptProtectData or X509. Personal certificates can be created:

```powershell
New-SelfSignedCertificate -KeySpec KeyExchange -KeyExportPolicy Exportable -HashAlgorithm SHA1 -KeyLength 2048 -CertStoreLocation "cert:\CurrentUser\My" -Subject "CN=MyRDCManCert"
```
