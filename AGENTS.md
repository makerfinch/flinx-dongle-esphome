# Instructions for coding agents

Read `README.md` first. It holds the hardware, the protocol, the flashing steps and what is proven.
This file is the rules for working on it.

## This drives a garage door

- **Never send a door command unless a person is at the door and has asked for it.** Open, close, stop,
  partial open and the light are physical actions. A test is a person watching, not a log line.
- **Prove a change by reading frames, not by moving the door.** Status frames, echoes and the `uart:
  debug:` log show whether the link works.
- **Never reorder or batch commands into a timed sequence.** One command, then check what happened.

## The hardware can be damaged

- The opener's socket has **24 V on VBUS**. Never have the dongle in the socket while anything is
  wired to its `DEBUG` header.
- The `DEBUG` header is **3.3 V only**. Tell the user to measure the adapter's output first.
- Do not try to reflash or talk to the **STC8G1K08**. Its firmware is not backed up and it is the part
  that talks to the motor.

## Secrets

- A stock firmware dump holds the owner's Wi-Fi password and the per-device AES key. **Never commit a
  `.bin`, a dump, or console output from `AT+KEY?`.** `*.bin` is gitignored; keep it that way.
- `secrets.yaml` is gitignored. Only `secrets.example.yaml` goes in git.
- Never pair a reflashed or restored dongle with the F-linX app on the user's behalf. Its id is tied to
  someone's cloud account.

## Stock firmware console

On the stock firmware, UART0 is a console. Read-only: `AT+KEY?`, `AT+BLENAME?`, `AT+WIFIMAC?`,
`nvs_list nvs`. **Never type** `reset`, `wifi`, `pair` or `study`.

## Building

- Use `uvx esphome@2026.9.1`, the version this config is proven on. Bump deliberately and say so.
- `esphome upload` re-flashes the previous binary without compiling. Use `esphome run`, or `compile`
  then `upload`.
- [`uartex`](https://github.com/eigger/espcomponents) is pinned to `v260904`. Older pins do not compile on 2026.9.
- Logs come over the API (`esphome logs --device <ip>`). The header is silent under ESPHome.

## Keeping the docs honest

- Anything marked **unproven** in the README (partial open, the light command) stays marked until a
  person has seen it work on a door. Then update the README in the same change.
- Credit stays as written in "Credits". New findings from others get credited there too.
- Every mention of another repository or the forum thread is a link, every time it appears.
- The README is read by people who have not seen the other repositories. Keep it usable on its own:
  explain a term the first time it appears, and keep the reference material under "How it works".
- A finding goes in the README once, in the section it belongs to. Do not repeat it.
