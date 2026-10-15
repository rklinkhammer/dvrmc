# DVRMC

Vivado board integration for the DMOTOR controller on the Digilent Arty Z7-20
(`xc7z020clg400-1`). The saved project uses Vivado 2026.1, a 100 MHz processor
fabric clock, and a 20 MHz motor-controller clock.

## Project contents

- `dvrmc.xpr`: saved Vivado project.
- `dvrmc.srcs/sources_1/bd/design_1/`: block design and IP configuration.
- `dvrmc.srcs/constrs_1/imports/digilent-xdc/Arty-Z7-20-Master.xdc`:
  board constraints, including pull-downs on the four external fault inputs.

Generated bitstreams, runs, caches, simulation outputs and packaged IP are not
tracked. The imported XDC is the project's constraint source; changes to the
original file in a separate `digilent-xdc` checkout do not update this copy.

## Dependencies and opening

Place this checkout beside [dmotor](https://github.com/rklinkhammer/dmotor):

```text
workspace/
  dvrmc/
  dmotor/
```

Generate/export the current DMOTOR IP following `dmotor/docs/current_ip_export.md`.
The project expects these repositories:

```text
../dmotor/build/current_ip/mailbox_control/ip
../dmotor/build/current_ip/shared_engine/ip
../dmotor/build/current_ip/controller_axi/ip
```

Open `dvrmc.xpr` in Vivado 2026.1. Regenerate block-design output products and the
HDL wrapper if requested; the wrapper is a generated project output. Review IP
status before upgrading any components.

## EDF hardware integration

With the sibling `edf-dev` environment configured, build and import a matched
bitstream/XSA/device-tree bundle using:

```sh
cd ../edf-dev
./edf --target arty-z7-20 hardware --project ../dvrmc/dvrmc.xpr
```

See `edf-dev/docs/arty.md` for Linux image generation and FPGA Manager loading.
The hardware command builds an isolated copy of the saved project. Save project
and constraint changes before invoking it.

The 2026-10-14 batch build passed routed timing and DRC error checks, and its I/O
report confirmed all four fault-input pull-downs. This does not establish physical
motor qualification; see DMOTOR's integration and protection requirements.
