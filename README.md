# nmosfet_synopsys_tcad
# 3D nMOSFET (45 nm Gate) Short-Channel Study: Sentaurus TCAD

A parameterised 3D n-channel MOSFET built and simulated in **Synopsys Sentaurus TCAD (V-2024.03)**. The project examines how the gate controls a 45 nm channel, using Id-Vgs sweeps at low and high drain bias to observe **DIBL** (drain-induced barrier lowering).

## Device

| Parameter | Value |
|---|---|
| Gate length, Lgate | 0.045 µm (Workbench parameter `Lgate`) |
| Width, W | 0.1 µm |
| Substrate depth | 0.3 µm |
| Gate oxide (SiO2) | 3 nm |
| Poly-Si gate thickness | 50 nm |
| Source/drain junction depth | 20 nm |
| Body doping (boron) | 2e18 cm^-3 |
| Source/drain doping (arsenic) | 1e20 cm^-3 |
| Contacts | source, drain, gate, substrate |

Doping is uniform (box profiles) for mesh robustness, which simplifies real graded junctions. The substrate spans x = -0.5 to 0.5 µm, so the geometry is set up for short gate lengths. Longer gates (about 0.8 µm and above) need a wider substrate.

## Simulation setup

- **Tools:** Sentaurus Workbench, Structure Editor (SDE, snmesh), Sentaurus Device, Sentaurus Visual.
- **Models:** drift-diffusion, doping-dependent mobility, high-field saturation, normal-field mobility degradation, SRH recombination. No quantum effects, tunnelling or self-heating.
- **Bias sequence:** equilibrium, then Vds = 0.05 V with a gate sweep 0 to 1.2 V (linear), then gate back to 0 V, Vds = 1.0 V and a second gate sweep 0 to 1.2 V (saturation).
- **Vth extraction:** constant-current method at 1e-7 A.
- **DIBL** = (Vth,lin − Vth,sat) / 0.95 V.


## How to run

1. Copy the files into a Sentaurus Workbench project folder and open it in Workbench.
2. Check that the SDE tool uses `MOSFET3d_dvs.cmd` and the SDEVICE tool uses `sdevice_des.cmd`, and set `Lgate` to 0.045.
3. Preprocess (Ctrl+P), then run SDE, then SDEVICE.
4. In Sentaurus Visual, plot the gate `OuterVoltage` against the drain `TotalCurrent` (log scale) for `IdVgs_lin_*` and `IdVgs_sat_*`.

## Results
<img width="1600" height="900" alt="nmosfet_45nm_lin_sat" src="https://github.com/user-attachments/assets/1351b757-b5d6-4507-bc21-22b34ec044e0" />
<img width="1600" height="900" alt="nmosfet_45nm_graph" src="https://github.com/user-attachments/assets/d9f2cc62-dc89-4e8b-a8b1-bb50515d6d5b" />


![Id-Vgs at Vds = 0.05 V](results/idvgs_lin_0p045.png)

*Id-Vgs at Vds = 0.05 V on a log scale. The current falls by about four decades toward Vg = 0, so the gate controls the channel.*

At Vds = 1.0 V the transfer curve shifts to lower gate voltage and carries higher current than at Vds = 0.05 V, which is the signature of DIBL.

## Design notes

A first version with 1e17 cm^-3 body doping and 50 nm deep junctions showed less than one decade of current modulation over the full gate sweep, indicating sub-surface **punchthrough**. Raising the body doping to 2e18 cm^-3 and reducing the junction depth to 20 nm restored gate control. Results therefore depend on these choices and are specific to this structure.

## Limitations and possible extensions

- Uniform doping, no graded junctions or halo implants.
- Drift-diffusion only, no quantum or ballistic effects.
- A single gate length. Comparing several gate lengths (for example 0.1, 0.25, 0.5 and 1.0 µm) would give a Vth roll-off trend.

## Author

[lathifunnisa], Department of Electronics and Communication Engineering, Dhanekula Institute of Engineering & Technology, Vijayawada.


