````markdown

# Parcial – Modelización y Simulaciones (CC2017)

## Requisitos

- **Python 3.9+**
- Paquetes: `numpy`, `pandas`, `matplotlib`, `jupyter`

**Instalación rápida (Windows):**
```bat
py -m venv venv
venv\Scripts\activate
py -m pip install -r requirements.txt
````

> Si no tienes `requirements.txt`:
> `py -m pip install numpy pandas matplotlib jupyter`


## Uso del notebook

1. Abre Jupyter:

   ```bat
   jupyter notebook parcial_modsim.ipynb
   ```
2. En la celda **CONFIG**, ajusta:

   * `OUTDIR` (carpeta de salida, por defecto `./out`)
   * `N_BOOT`, `K_BINS`, `N_ACCEPT`, `SEED`
   * `RUN_D`, `RUN_E`, `RUN_F` (qué incisos ejecutar)
3. Menú **Kernel → Restart & Run All** para ejecutar todo.
4. Las figuras/CSVs se guardan en `OUTDIR` y puedes mostrarlas **inline** con `%matplotlib inline` + `plt.show()`.

**Mostrar y guardar gráficos en cada celda:**

```python
plt.savefig("ruta.png", dpi=160)  # guarda
plt.show()                        # muestra en el notebook
plt.close()
```

---

## Estructura mínima

```
.
├─ parcial_modsim_onefile.ipynb   # ÚNICO notebook
├─ requirements.txt               # Dependencias
├─ out/                           # Salidas generadas
│  ├─ hist_bootstrap_means.png
│  ├─ probabilidades_por_bin_k5.csv
│  ├─ pmf_simplificada_k5.png
│  ├─ cdf_simplificada_k5.png
│  ├─ pmf_simplificada_k5_f.png
│  ├─ cdf_simplificada_k5_f.png
│  ├─ hist_ar_simplified.png
│  ├─ bootstrap_resumen.csv
│  ├─ ar_metricas.csv
│  ├─ edges_k5.npy
│  └─ probs_bins_k5.npy
└─ README.md
```

> Si `out/` no existe, el notebook la crea automáticamente.

---

## Qué hace cada inciso

* **(d) Bootstrapping con Transformada Inversa**
  Re-muestrea con reemplazo usando la CDF discreta (10 probabilidades). Obtiene la **distribución de la media muestral**, histograma y **desviación estándar**.

* **(e) Rangos / PMF / CDF**
  Divide el rango observado de las medias en **K\_BINS** iguales; calcula **probabilidades por bin** (frecuencia relativa), **PMF** (barras en midpoints) y **CDF** (escalonada).

* **(f) Aceptación–Rechazo (GCLM)**
  Objetivo: distribución discreta de 5 puntos (midpoints de (e)). Propuesta **uniforme**; se usa **GCLM** clásico. Reporta **constante c**, **tasa de aceptación** e histograma + PMF/CDF.

---

## Checks de validación

**(d) Bootstrap de medias**

* Media teórica de las medias ≈ **1.11**
  (Prob. agregadas: $P(2)=0.53$, $P(1)=0.25$, $P(0)=0.17$, $P(-4)=0.05$)
* SD teórica de las medias ≈ **0.44**
  ($\sigma_X\approx 1.391$, $\sigma_{\bar X}=\sigma_X/\sqrt{10}$)
* Rango teórico de $\bar X$: $[-4,\,2]$

**(e)**

* `probabilidades_por_bin_k5.csv` debe **sumar 1.0**
* PMF con barras en midpoints; CDF no decreciente y termina en **1**

**(f)**

* $c = 5 \cdot \max(p_i)$ → típico ≈ **2.334**
* Tasa de aceptación ≈ $1/c$ → típico ≈ **0.428**
* Proporciones por bin en la muestra A\&R ≈ $p_i$

> Para reproducibilidad exacta: `np.random.seed(42)` en (d) y fija `SEED` en (f).

---

## Problemas frecuentes
* **CSV de bins no suma 1**: confirma que `K_BINS` y los archivos `edges_k*.npy` / `probs_bins_k*.npy` provienen de la **misma corrida**.
* **A\&R lento**: si `c` es grande, la aceptación baja; ajusta `N_ACCEPT` o revisa los `p_i`.
* **Errores de ruta**: usa rutas relativas (por defecto `./out`) y verifica permisos.

---

## 📄 Autor / Licencia

* Autor: **José Auyón**
* Uso: **Académico**
* Contacto: **[auy201579@uvg.edu.gt](auy201579@uvg.edu.gt)**


