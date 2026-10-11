# Mega Eufy HACS Custom Build

Custom build of the Mega Eufy Home Assistant integration for distribution through HACS.

The integration is a client. It needs the matching **eufy-sdk bridge** add-on from the companion
custom add-on repository. Install both parts from the Tralla custom-build repositories below.

## Custom build versus upstream

This custom build is published separately from the upstream `mega-yfue` project. Use these two
repositories together:

| Part | Repository | Where it is added |
| --- | --- | --- |
| Integration | `https://github.com/Tralla/custombuild-ha-eufy-sdk` | HACS → Custom repositories, type **Integration** |
| Bridge add-on | `https://github.com/Tralla/custombuild-ha-eufy-sdk-addon` | Home Assistant add-on store → Repositories |

Do not mix this build with the upstream repositories `mega-yfue/ha-eufy-sdk` or
`mega-yfue/ha-eufy-sdk-addon`. Some text inside the integration and the add-on still links to
upstream, because those files come from upstream source; follow the instructions on this page
instead. In particular, do not add the upstream add-on repository: its add-on has the same name,
**eufy-sdk bridge**, but runs the upstream Bridge image rather than this build's.

You can recognise the custom-build add-on by its version, which contains `custom` (for example
`<version>-custom.<date>.<n>`), and by the repository it was installed from. The `custom` marker
appears in the HACS release tag and in the add-on version; Home Assistant's integration information
shows the plain manifest version, without `custom`. Install the integration and add-on from
releases with the same date stamp, normally the latest of each.

## Requirements

- Home Assistant OS, which provides the add-on store. Home Assistant Container
  and Core installations cannot run add-ons, and this page does not cover running the Bridge
  without the add-on.
- [HACS](https://hacs.xyz/) installed. HACS enforces the minimum Home Assistant version declared by
  this repository.
- A supported architecture for the add-on, currently `amd64` or `aarch64`.
- A eufy account. eufy allows one active session per account: run only one Bridge for an account,
  or the Bridges will keep signing each other out.

## Install

This procedure follows the tested manual installation path. Menu names can differ slightly between Home
Assistant versions; newer releases call add-ons **Apps**.

### 1. Install the integration from HACS

1. In HACS, open the **⋮** menu and choose **Custom repositories**.
2. Add `https://github.com/Tralla/custombuild-ha-eufy-sdk` with type **Integration**.
3. Open the **eufy-sdk** repository in HACS and download it.
4. Restart Home Assistant when prompted.

### 2. Install the Bridge add-on

1. Open **Settings → Add-ons → Add-on Store**, then the **⋮** menu → **Repositories**.
2. Add `https://github.com/Tralla/custombuild-ha-eufy-sdk-addon`.
3. Install **eufy-sdk bridge** from that repository. Check that its version contains `custom`.
   Ignore **eufy-sdk bridge (dev)**: it is an experimental upstream channel, not part of this build,
   and is only shown when Advanced Mode is enabled.
4. On the add-on **Configuration** tab, set `email`, `password`, and `country` (two-letter code of
   your eufy account region). The remaining options can stay at their defaults; the add-on's
   **Documentation** tab explains them.
5. Start the add-on and check its **Log** tab for a successful start.

### 3. Add the integration

1. Open **Settings → Devices & services → Add integration** and choose **eufy-sdk**.
2. Enter the Bridge connection settings:

   | Field | Default | When to change it |
   | --- | --- | --- |
   | Host | `homeassistant.local` | Use your Home Assistant host's name or IP address if `homeassistant.local` does not resolve. |
   | Port | `3000` | Use the host port shown for `3000/tcp` on the add-on's **Network** panel if you changed it. |
   | RTSP port | `8554` | Use the host port shown for `8554/tcp` on the add-on's **Network** panel if you changed it. |

   Use `homeassistant.local` or your Home Assistant host's IP address. `localhost` and
   `127.0.0.1` are not the documented values.
3. If eufy asks for a verification code or a captcha, complete it in the integration dialog.
4. After setup, your eufy devices appear as devices and entities. Which entities appear depends on
   what each device reports.

## Automatic discovery (not yet verified for this build)

The add-on is designed to announce itself to Home Assistant after it starts. Home Assistant may
then offer a discovered **eufy-sdk** integration under **Settings → Devices & services**, with the
host and ports already filled in. This path exists in the code but has not been tested for this
custom build. If no discovered integration appears, use the manual setup in step 3.

## Optional shortcuts (not yet tested)

These My Home Assistant links may shorten the repository steps. They have not been tested for this
build. Each opens your own Home Assistant instance and asks you to confirm before anything is added.
If a link does not work, use the manual steps above.

- Open this repository in HACS:
  [![Open your Home Assistant instance and open a repository inside the Home Assistant Community Store.](https://my.home-assistant.io/badges/hacs_repository.svg)](https://my.home-assistant.io/redirect/hacs_repository/?owner=Tralla&repository=custombuild-ha-eufy-sdk&category=integration)
- Add the custom add-on repository:
  [![Open your Home Assistant instance and show the add add-on repository dialog with a specific repository URL pre-filled.](https://my.home-assistant.io/badges/supervisor_add_addon_repository.svg)](https://my.home-assistant.io/redirect/supervisor_add_addon_repository/?repository_url=https%3A%2F%2Fgithub.com%2FTralla%2Fcustombuild-ha-eufy-sdk-addon)

## Updates

Update the integration through HACS and the add-on through the add-on store. Keep both components
on the Tralla custom-build releases, identified by the `custom` version marker. Prefer integration
and add-on releases with matching date stamps. Do not mix custom builds with upstream releases. The
update path has not yet been separately tested for this build.
