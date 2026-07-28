# eez-rigol-dho924s

EEZ Studio extension for the **Rigol DHO924S** — 4-channel, 200 MHz, 12-bit
oscilloscope, 2 GSa/s sample rate, up to 200 Mpts memory depth. Also covers
the DHO914S (100 MHz 12-bit), DHO924 (200 MHz 8-bit), and DHO914 (100 MHz
8-bit) — same command family.

Like its siblings [eez-keysight-34465a](https://github.com/ksstech/eez-keysight-34465a)
and [eez-rigol-mho98](https://github.com/ksstech/eez-rigol-mho98), and unlike
[eez-ea-ps2k](https://github.com/ksstech/eez-ea-ps2k), this instrument speaks
**native SCPI** directly — no custom bridge process.

This is the simplest of the four extensions in this family — 7 shortcuts,
all plain `scpi-commands` (no JavaScript, no shared helpers needed).

## Connection

- **Ethernet (LXI), recommended:** port `5555`. Enter the instrument's IP
  address when adding it in EEZ Studio.
- **USB-TMC:** `idVendor 0x1ab1`, `idProduct 0x0515`.

Verify the `*IDN?` prefix on your firmware — some units return `RIGOL
TECHNOLOGIES` rather than `Rigol Technologies`; adjust EEZ Studio's IDN
match if auto-detection fails.

## Structure

| Path | Purpose |
|---|---|
| `package.json` | Extension metadata + 7 shortcuts |
| `rigol_dho924s.idf` | EEZ Studio instrument definition |
| `rigol_dho924s.sdl` | SCPI command/response definitions |

No `image.png` currently included (unlike the other three extensions in
this family) — worth adding one for a consistent icon in EEZ Studio's
instrument list.

Built as a zip and published via [GitHub Releases](https://github.com/ksstech/eez-rigol-dho924s/releases) — not committed to the repo.

## Functionality — shortcuts

All toolbar, all plain SCPI commands: `Run`, `Stop`, `Single`, `Autoset`,
`Screenshot`, `Status`. Plus `Reset` (hidden from toolbar, requires
confirmation).

## Using it without EEZ Studio

Native SCPI — works with any SCPI client already; this repo only adds
convenience shortcuts on top.

**Raw socket (LAN SCPI, port 5555):**
```bash
echo -e "*IDN?\n" | nc 192.168.1.51 5555
# RIGOL TECHNOLOGIES,DHO924S,...
```

**Python via PyVISA:**
```python
import pyvisa
rm = pyvisa.ResourceManager()
scope = rm.open_resource("TCPIP0::192.168.1.51::5555::SOCKET")
scope.read_termination = "\n"
scope.write_termination = "\n"

print(scope.query("*IDN?"))
scope.write(":RUN")
```

## License

No LICENSE file is currently set — add one if this is meant to be reused
under specific terms; until then, standard GitHub default (all rights
reserved) applies to original content here.
