# TFMCodigoDeDiferentesCuanticos



# TFM — Código de Diferentes Simuladores Cuánticos

Este repositorio reúne implementaciones y scripts de evaluación para los algoritmos **Deutsch–Jozsa**, **Bernstein–Vazirani** y **Grover** sobre distintos **simuladores cuánticos**: **Qiskit Aer**, **Cirq** y (opcionalmente) **Microsoft QDK**. El objetivo es facilitar la comparación de **tiempos de ejecución** y **uso de memoria**, así como reproducir figuras y tablas del informe.

## 🧭 Contenidos

* `qiskit/` — circuitos y utilidades en Qiskit (Aer).
* `cirq/` — circuitos y utilidades en Cirq.
* `qdk/` — (opcional) implementaciones en Q# / QDK.
* `benchmarks/` — scripts de escalabilidad (tiempo/memoria vs. qubits).
* `figures/` — gráficos `.pdf/.png` generados.
* `data/` — resultados en `.csv` / `.json`.
* `envs/` — ficheros de entorno (`requirements.txt`, `environment.yml`).
* `README.md` — este archivo.

> Si tu estructura actual es distinta, puedes crear estas carpetas y mover tus scripts; el README ya contempla comandos genéricos.

---

## 🔧 Requisitos e instalación

Se recomienda **Python 3.10+** y un **entorno virtual**.

### 1) Crear entorno

```bash
# con venv
python -m venv .venv
# Linux/Mac
source .venv/bin/activate
# Windows (PowerShell)
.venv\Scripts\Activate.ps1
```

### 2) Instalar dependencias mínimas

#### Qiskit (Aer)

```bash
pip install qiskit qiskit-aer
```

> Aer se instala explícitamente con `pip install qiskit-aer`. Para importar, usa `from qiskit_aer import AerSimulator`. ([qiskit.github.io][2], [Quantum Computing Stack Exchange][3])

#### Cirq

```bash
pip install cirq
```

> Guía oficial: instalar Cirq con `pip install cirq`. ([Google Quantum AI][4], [PyPI][5])

#### (Opcional) Microsoft QDK / Q\#

```bash
pip install qsharp azure-quantum
```

> QDK moderno vía `qsharp` y (si aplicas Azure) `azure-quantum`. ([Microsoft Learn][6], [quantum.microsoft.com][7])

> **Tip GPU (opcional):** Qiskit Aer tiene paquete con soporte GPU (consulta requisitos CUDA en la doc de Aer). ([qiskit.github.io][2], [PyPI][8])

---

## ▶️ Ejecución rápida

Cada subcarpeta contiene scripts de ejemplo para los tres algoritmos. La interfaz típica es:

```bash
# Qiskit Aer
python qiskit/deutsch_jozsa.py --n 10
python qiskit/bernstein_vazirani.py --n 10 --secret 10110110
python qiskit/grover.py --n 8 --marked 00010110

# Cirq
python cirq/deutsch_jozsa.py --n 10
python cirq/bernstein_vazirani.py --n 10 --secret 10110110
python cirq/grover.py --n 8 --marked 00010110

# QDK (Q# + Python)
python qdk/deutsch_jozsa.py --n 10
```

**Parámetros comunes**

* `--n` : número de qubits lógicos del registro de entrada.
* `--shots` : (si aplica) repeticiones para histogramas.
* `--seed` : semilla para reproducibilidad.

> Si los nombres de archivo difieren, ajusta los comandos al nombre real de tus scripts.

---

## 📊 Benchmarks y escalabilidad

Los scripts de `benchmarks/` permiten medir **tiempo** y **memoria** en función de `--n` (qubits). Ejemplos:

```bash
# Tiempo/memoria para Qiskit Aer (cadena de CNOT)
python benchmarks/run_scalability.py --backend qiskit --algo cnot_chain --min_n 5 --max_n 30 --step 5

# Cirq (misma topología)
python benchmarks/run_scalability.py --backend cirq --algo cnot_chain --min_n 5 --max_n 30 --step 5

# (Opcional) QDK: usar Deutsch–Jozsa si el optimizador Clifford elimina puertas redundantes
python benchmarks/run_scalability.py --backend qdk --algo deutsch_jozsa --min_n 6 --max_n 16
```

Los resultados se guardan en `data/` (`.csv`) y los gráficos en `figures/` (por ejemplo, **tiempo vs qubits** en escala log y **memoria vs qubits**).
Incluye también scripts para generar figuras con **pgfplots/LaTeX** si quieres integrarlas directamente en tu TFM.

---

## 🧪 Ejemplos mínimos

### Qiskit Aer (Python)

```python
from qiskit import QuantumCircuit, transpile
from qiskit_aer import AerSimulator

qc = QuantumCircuit(2, 2)
qc.h(0); qc.cx(0,1); qc.measure([0,1],[0,1])

backend = AerSimulator()
job = backend.run(transpile(qc, backend), shots=1024)
print(job.result().get_counts())
```

### Cirq (Python)

```python
import cirq
q0, q1 = cirq.LineQubit.range(2)
circuit = cirq.Circuit(cirq.H(q0), cirq.CNOT(q0, q1), cirq.measure(q0, q1))
sim = cirq.Simulator()
print(sim.run(circuit, repetitions=1024))
```

### QDK/Q# (Python + qsharp)

```python
import qsharp
# en Jupyter también puedes usar %%qsharp para pegar directamente código Q#
```



