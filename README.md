# 🔍 Understanding the CALC_MODES

I have performed a linear buckling analysis (also known as eigenvalue **buckling** analysis). This predicts the load levels at which the structure becomes unstable, due to torsional buckling. This is done by solving the generalized eigenvalue problem:

### (K - λ * K<sub>G</sub>) * φ = 0

Where:

- **K**: Mechanical stiffness matrix (linear stiffness)
- **K<sub>G</sub>**: Geometric stiffness matrix (accounts for pre-stress effects)
- **λ**: Buckling multiplier (critical load factor)
- **φ**: Buckling mode shape (eigenvector)

### Understanding the terms:

```python
modes = CALC_MODES(
    CALC_CHAR_CRIT=_F(
        NMAX_CHAR_CRIT=12,
        PREC_SHIFT=0.005,
        SEUIL_CHAR_CRIT=0.01),
```

### 🔍 What it does:

- **`CALC_CHAR_CRIT`**: Block used to extract **buckling load factors** (critical eigenvalues).

- **`NMAX_CHAR_CRIT=12`**: Computes up to **12 buckling modes**.

- **`PREC_SHIFT=0.005`**: Used in **shift-and-invert** strategies to enhance numerical convergence; this sets the precision of the shift.

- **`SEUIL_CHAR_CRIT=0.01`**: Sets a **threshold** for eigenvalues; modes with buckling factors **below this value** are treated as **non-physical or numerically insignificant** and are ignored.


```python
    MATR_RIGI=KM,
    MATR_RIGI_GEOM=KG,
```

- **`MATR_RIGI`**:Reference to the mechanical stiffness matrix (KM).

- **`MATR_RIGI_GEOM:`**:  Reference to the geometric stiffness matrix (KG), based on stress state from previous static analysis (SIEF_ELGA field).


```python
    OPTION='PLUS_PETITE',
```
- Find the smallest positive eigenvalues, which correspond to the lowest (critical) buckling loads — the most important in safety analysis.

```python
    SOLVEUR_MODAL=_F(
        DIM_SOUS_ESPACE=200,
        METHODE='SORENSEN',
        NMAX_ITER_SOREN=100,
        PREC_SOREN=1e-08),
```
-  **`MATR_RIGI`**:Reference to the mechanical stiffness matrix (KM).
-  **`DIM_SOUS_ESPACE=200`**: Size of the Krylov subspace used in the solver.
-  **`METHODE='SORENSEN'`**: Use Sorensen's implicitly restarted Arnoldi method, which is good for a few smallest eigenvalues.
-  **`NMAX_ITER_SOREN=100`**: Max number of iterations to converge.
-  **`PREC_SOREN=1e-08`**: Precision of the solution, aiming for a highly accurate eigenvalue (buckling factor).

```python
    TYPE_RESU='MODE_FLAMB',
```
- Specifies the type of result: **`MODE_FLAMB`** = buckling mode result.

```python
        VERI_MODE=_F(
        PREC_SHIFT=0.005,
        SEUIL=1e-05,
        STOP_ERREUR='NON',
        STURM='OUI'))
```

- **`PREC_SHIFT=0.005`**: Used in verifying the shift used for buckling mode accuracy.
- **`SEUIL=1e-05`**: Eigenvalues smaller than this are ignored as numerical artifacts.
- **`STOP_ERREUR='NON'`**: Do not stop the analysis if validation fails.
- **`STURM='OUI'`**: Use Sturm sequence check to verify correct number of eigenvalues in the specified range.



