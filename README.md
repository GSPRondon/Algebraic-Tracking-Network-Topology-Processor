# Algebraic-Tracking-Network-Topology-Processor
This repository contains the basic scripts for executing the Algebraic Tracking Network Topology Processor (AT-NTP) proposed in the paper "Algebraic Tracking Network Topology Processor for Modern Power Systems" by G. S. P. Rondon, V. H. P. de Melo, A. C. Mouco and J. B. A. London Jr, submitted to the IEEE Access journal. The main purpose of the code is to obtain the topology of electrical networks in the bus-branch model (BBM) based on the factorization of system matrices.

# Versions of the Code
Two versions of the code are available: “AT_NTP_Original” and “AT_NTP_Comparative_Tests”.
The first version corresponds to the full implementation of the AT-NTP, including the stages of Substation-Level Processing (SLP), Network-Level Processing (NLP), and Measurement Arrangement Processing (MAP).
The second version is used to perform comparative studies with other Network Topology Processors (NTPs) found in the literature and does not include the MAP stage, since this step is not carried out by the NTPs selected for comparison.

# Description of Input Files

The first file read by the program is **`Configuracao.txt`**, in which the user must specify the name of the folder containing the electrical system to be simulated.  
Inside each folder, the following files are present:

---

### 🧾 `DARRANJ.csv`
Contains **measurement arrangement data**.

| Column | Description |
|:-------:|--------------|
| 1 | Meter ID |
| 2 | Meter type (0 – Active/reactive power measurement pair; 1 – Voltage measurement; 2 – Current measurement) |
| 3 | Measurement instrument type (0 – Potential Transformer (PT); 1 – Current Transformer (CT)) |
| 4 | “From” bus section ID |
| 5 | “To” bus section ID |
| 6 | For CTs, indicates the type of connected equipment (0 – Transmission line; 1 – Power transformer; 2 – Load; 3 – Other shunt equipment; 4 – Generating unit) |
| 7 | “To” bus section ID after transmission line/transformer (if Column 6 = 0 or 1) |
| 8 | Shunt element ID (if Column 6 = 2, 3, or 4) |

---

### 🧾 `DBAR.csv`
Contains **bus section data**.

| Column | Description |
|:-------:|--------------|
| 1 | Substation ID |
| 2 | Bus section ID |

---

### 🧾 `DCHAV.csv`
Contains data on **Switchable Devices (SDs)** in the network topology used for AT-NTP initialization.

| Column | Description |
|:-------:|--------------|
| 1 | “From” bus section ID |
| 2 | “To” bus section ID |
| 3 | SD status (0 – Open; 1 – Closed) |

---

### 🧾 `DCHAV_ATUALIZADO.csv`
Contains data on **SDs in the updated network topology** (same structure as `DCHAV.csv`).

---

### 🧾 `DGEN.csv`
Contains **generating unit data**.

| Column | Description |
|:-------:|--------------|
| 1 | Generator unit ID |
| 2 | Bus section ID to which the generator is connected |
| 3 | Generator status (0 – Deactivated; 1 – Activated) |

---

### 🧾 `DLOADS.csv`
Contains **load data**.

| Column | Description |
|:-------:|--------------|
| 1 | Load ID |
| 2 | Bus section ID to which the load is connected |
| 3 | Load status (0 – Deactivated; 1 – Activated) |

---

### 🧾 `DRAM.csv`
Contains **network branch data**.

| Column | Description |
|:-------:|--------------|
| 1 | Branch type (0 – Transmission line; 1 – Transformer) |
| 2 | “From” bus section ID |
| 3 | “To” bus section ID |

---

### 🧾 `DSHUNT.csv`
Contains data on **other shunt devices** in the network.

| Column | Description |
|:-------:|--------------|
| 1 | Shunt device ID |
| 2 | Shunt device type (0 – Synchronous condenser; 1 – Capacitor bank) |
| 3 | Bus section ID to which the shunt device is connected |
| 4 | Shunt device status (0 – Deactivated; 1 – Activated) |

---

> ⚙️ **Note:**  
> These input files contain only **connectivity data**, which are effectively processed by the **AT-NTP**.  
> Additional information such as **branch parameters**, **shunt device parameters**, and **measured values** are **not processed by most Network Topology Processors (NTPs)** and were therefore omitted.  
> However, these files can be easily **extended to include additional elements** of electrical power systems.

# Description of Output Files

After execution, the program generates several **output files** corresponding to the results of the **Algebraic Tracking Network Topology Processor (AT-NTP)**.  

---

### 🧾 `DBAR_CR.csv`
Contains **bus data** of the **Bus-Branch Model (BBM)** used for the initialization of the AT-NTP.

| Column | Description |
|:-------:|--------------|
| 1 | Substation ID |
| 2 | Equivalent bus ID in the BBM |

---

### 🧾 `DBAR_TR.csv`
Contains **bus data** of the **updated BBM** of the network.  
(Same structure as `DBAR_CR.csv`.)

---

### 🧾 `DGEN_CR.csv`
Contains **generating unit data** of the **BBM used for AT-NTP initialization**.

| Column | Description |
|:-------:|--------------|
| 1 | Generator unit ID |
| 2 | BBM bus ID to which the generator unit is connected |
| 3 | Generator status (0 – Deactivated; 1 – Activated) |

---

### 🧾 `DGEN_TR.csv`
Contains **generating unit data** of the **updated BBM** of the network.  
(Same structure as `DGEN_CR.csv`.)

---

### 🧾 `DLOADS_CR.csv`
Contains **load data** of the **BBM used for AT-NTP initialization**.

| Column | Description |
|:-------:|--------------|
| 1 | Load ID |
| 2 | BBM bus ID to which the load is connected |
| 3 | Load status (0 – Deactivated; 1 – Activated) |

---

### 🧾 `DLOADS_TR.csv`
Contains **load data** of the **updated BBM** of the network.  
(Same structure as `DLOADS_CR.csv`.)

---

### 🧾 `DMED_CR.csv`
Contains **measurement data** allocated in the **BBM used for AT-NTP initialization**.

| Column | Description |
|:-------:|--------------|
| 1 | Measurement type (0 – Power flow; 1 – Power injection; 2 – Current flow; 3 – Current injection; 4 – Voltage) |
| 2 | “From” bus ID of the measurement in the BBM |
| 3 | “To” bus ID of the measurement in the BBM (only for flow measurements) |

---

### 🧾 `DMED_TR.csv`
Contains **measurement data** allocated in the **updated BBM** of the network.  
(Same structure as `DMED_CR.csv`.)

---

### 🧾 `DRAM_CR.csv`
Contains **branch data** of the **BBM used for AT-NTP initialization**.

| Column | Description |
|:-------:|--------------|
| 1 | Branch type (0 – Transmission line; 1 – Transformer) |
| 2 | “From” bus ID in the BBM |
| 3 | “To” bus ID in the BBM |

---

### 🧾 `DRAM_TR.csv`
Contains **branch data** of the **updated BBM** of the network.  
(Same structure as `DRAM_CR.csv`.)

---

### 🧾 `DSHUNT_CR.csv`
Contains **shunt equipment data** of the **BBM used for AT-NTP initialization**.

| Column | Description |
|:-------:|--------------|
| 1 | Shunt device ID |
| 2 | Shunt device type (0 – Synchronous condenser; 1 – Capacitor bank) |
| 3 | BBM bus ID to which the device is connected |
| 4 | Indicates whether there is a power injection measurement linked to this device (0 – No; 1 – Yes) |
| 5 | Indicates whether there is a current injection measurement linked to this device (0 – No; 1 – Yes) |

---

### 🧾 `DSHUNT_TR.csv`
Contains **shunt equipment data** of the **updated BBM** of the network.  
(Same structure as `DSHUNT_CR.csv`.)

---

### 🧾 `Tempos.csv`
Contains **execution times** of the AT-NTP (all in seconds).

| Column | Description |
|:-------:|--------------|
| 1 | AT-NTP initialization time |
| 2 | Time for BBM update by the AT-NTP |
| 3 | Time of the NLP and MAP stages during update |

---

> ⚙️ **Note:**  
> These output files provide the results of the AT-NTP stages and allow for the verification of network topology updates, element status changes, and overall processing times.

# Available Power Systems

In the **“AT_NTP_Original”** version, the following folders are available:  
**S14B**, **S39B**, **S118B**, **S300B**, and **S9241B**, containing data from the **IEEE 14-, 39-, 118-, and 300-bus systems**, and from the **9241-bus network**, respectively.

---

In the **“AT_NTP_Comparative_Tests”** version, the following systems are available:  
the **IEEE Reliability Test System 1996** and the **modified two-area, four-machine power system**.

- The **IEEE Reliability Test System 1996** data are available in the folders **S24B1**, **S24B2**, and **S24B3**, which are used, respectively, for simulating **scenarios 1, 2, and 3** described in the paper *“An efficient automated topology processor for state estimation of power transmission networks.”*

- The **modified two-area, four-machine power system** data are available in the folders **S20B1**, **S20B2**, and **S20B3**, which are used, respectively, for simulating **scenarios 1, 2, and 3** described in the paper *“An efficient and reliable electric power transmission network topology processing.”*

# How to Run

It is recommended to run the code on a **Linux operating system**.  
Due to the libraries used for execution time measurement, **errors may occur on Windows**.

Make sure that **Meson** and **Ninja Build** are installed.  
Run the following command in the terminal:

```bash
sudo apt install meson ninja-build
```
With Meson installed and configured, open the terminal in the program’s root folder and execute the following script:
```bash
bash run.sh
```
