
# ChipWhisperer Datasets

These datasets are side-channel power analysis (and maybe other eventually?) traces. They are designed to be used in evaluating various algorithms by providing a standard comparison base.

The end goal is having captures done with:

* Various underlying hardware (Cortex-M4 from Vendor A, B, then PowerPC, etc etc).
* Various encryption algorithms (AES, RSA, ECC).
* Various encryption libraries (hardware peripheral on chip, open-source versions, etc).

Note that is an *end goal* and it currently focuses on AES only. More coming soon...

### Hardware Platforms

The hardware in the [ChipWhisperer CW308 Targets](https://www.github.com/newaetech/chipwhisperer-cw308t-targets) repository is used as a list of available targets. The initial run will use the following target boards:

* STM32F3
* SAML11 (has hardware AES)
* SAM4L (has hardware AES)
* MK82F (has hardware AES)
* STM32L5 (has hardware AES)
* STM32F4 (has hardware AES)
* STM32L4  (has hardware AES)
* MPC5777C (has hardware AES)


## Python Interface

The datasets are currently designed to be used with Python, but you can easily use a small shim to copy them into your own file format. The advantage of the Python-based implementation is you can use cloud resources such as Google Co-Lab to perform either (a) a conversion if you want another format, or (b) the entire analysis.

The underlying dataset is stored in a Google Cloud Storage element. Usage from other Google Cloud resources is much faster!

## Host & Storage Costs

T.B.D. if this is expensive it might become invite-only or something, or only some of the datasets are accessible.

## Usage Example

Here is an example of how we hope you can use this (TODO - implement everything):

	import chipwhisperer-datasets as cwd


	cwd.list_datasets()

		CW308_STM32F3-AESECB-MBEDTLS-FK0RI-SH-SYNC-01:
           Platform      : NAE-CW308T-STM32F3
           Board SN      : LN000289
           Chip          : STM32F303RCT6 (Date-Code: 1414)
           Crypto        : AES-ECB
           Library       : MBED-TLS
           Key Pattern   : Fixed Key 0
           Input Pattern : Random
           Traces        : 1E5
           Points/Trace  : 1E4
           Measurement   : Shunt Resistor
           Taget CPU     : 8 MHz
           Capture       : 32 MHz (Syncronous)
           Coverage      : AES, 2 rounds

		...many more datasets...	

	cwd.list_platforms()

		...list of platforms...

	data = cwd.get_dataset('CW308T_STM32F3-AESECB-MEDTLS-FK2-RI-1E5-SH-SYNC-01')

The naming for datasets goes something like this:

	<PLATFORM NAME>-        Name of Hardware Target (i.e., CW308T_STM32F3 board)
	<CRYPTO>-               Crypto Algorithm (i.e., AES-ECB)
	<CRYPTO OPTIONS>-       How crypto is done (i.e., MBEDTLS, HW, etc)
	<KEY TYPE>-             Fixed Key #2 (FK2), Random Key (RK), etc
	<INPUT TYPE>-           Fixed Input #0 (FI0), Random Input (RI), etc
	<NUM TRACES>-           Number of traces in this set
	<SHUNT OR EM>-          Measurement taken with Shunt, EM, something else
	<SYNC>-                 Syncronous Measurement (ChipWhisperer)
	<N>-                    Variant number (in case we do multiple captures with same 'top-level' description).

The `FK2` means Fixed-Key, using "Key 2". There will be a number of specific fixed keys used that are standard between datasets (Key 2 is `2B7E...` for example). If no # is specified (i.e., just `FK`) it means some arbitrary fixed key is used, which is likely recorded in the dataset itself. You may want to try your algorithm with different fixed keys.

## Rebuilding Datasets

In order to increase repeatability, each dataset includes a copy of the hex-file programmed into the target device, along with some compiler output information.

In addition the git tag of the firmware version used is included such you could obtain a copy of the entire specific firmware source code.

The script used for capturing datasets is in the XXX folder.

## Backend Notes

TODO - backend.