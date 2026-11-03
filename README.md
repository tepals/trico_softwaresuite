# Trico Software Suite

This repository contains experimental software for use with Delmic's Odemis
microscope software. The interface displays fluorescence images, supports
region-of-interest (ROI) analysis and PSF fitting, and can control acquisition
and related microscope components.

This is not a completed or validated product. Use it only with compatible
hardware and follow your lab's operating and laser-safety procedures.

## Requirements

- Delmic Odemis installed and configured for the microscope.
- The Odemis backend running before you start this application. The app looks
  up the `align`, `focus`, `ccd`, `light`, and `filter` components at startup.
- Python dependencies available to the same Python interpreter that can import
  Odemis: PyQt5, pyqtgraph, NumPy, pandas, Matplotlib, SciPy, and Pillow.

Odemis is a separate platform dependency; install it using Delmic's instructions
for your system rather than treating it as a package from this repository.
On Ubuntu, install the Python dependencies from the system package manager:

```bash
sudo apt update
sudo apt install python3-numpy python3-pandas python3-pyqtgraph python3-pyqt5 \
  python3-matplotlib python3-scipy python3-pil
```

Run the app with Ubuntu's system Python (`python3`) so it can find these
packages. Ensure that this interpreter can also import the Odemis modules;
follow Delmic's installation instructions if Odemis is not available in the
system Python environment. There is no dependency lockfile or
`requirements.txt` yet.

## Start the interface

From the repository directory, with Odemis already running:

```bash
python3 AqImageNew.py
```

The main window provides image acquisition controls, continuous acquisition,
ROI plotting and fitting, stage/focus controls, filter and illumination
controls, and options to save image and metadata. Acquisition requires the
corresponding Odemis hardware components to be available.

## Note on camera configuration

Before acquiring data, verify that the camera configuration in
`AqImageNew.py` is appropriate for the connected hardware. The current code
sets the CCD resolution to `(204, 204)`, this allows it to work with the 
Odemis Enzel simulator. The UI's save controls write data
under the repository's `SavedData` directory or to the folder selected for a
run.

## Test with the Odemis simulator

The Enzel simulator configuration was deprecated after Odemis 3.7. Its YAML
file is available from the
[Odemis v3.7 branch](https://github.com/delmic/odemis/blob/v3.7-branch/install/linux/usr/share/odemis/sim/enzel-sim.odm.yaml).
Download it to a local directory, for example:

```bash
curl -L -o enzel-sim.odm.yaml \
  https://raw.githubusercontent.com/delmic/odemis/v3.7-branch/install/linux/usr/share/odemis/sim/enzel-sim.odm.yaml
```

Start the simulator backend from the directory containing the downloaded
file:

```bash
odemis-start enzel-sim.odm.yaml --nogui
```

With the backend running, open a second terminal in this repository and start
the interface as described above:

```bash
python3 AqImageNew.py
```

On current Odemis versions, the simulator GUI may not work, but the backend
can still run. The app connects to the running backend, so use `--nogui` and
leave that process running while testing.
