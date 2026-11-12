# High-Performance Python & NumPy Numerical Foundations for Data Science

[![Python 3.10+](https://img.shields.io/badge/Python-3.10%2B-blue.svg)](https://www.python.org/)
[![NumPy](https://img.shields.io/badge/NumPy-Vectorization-013243.svg)](https://numpy.org/)
[![IEEE 754](https://img.shields.io/badge/Standard-IEEE%20754%20Precision-informational.svg)](https://en.wikipedia.org/wiki/IEEE_754)
[![Institution](https://img.shields.io/badge/MSc-Data%20Science%20%26%20AI%20for%20Business-red.svg)](https://www.polytechnique.edu/)
[![Schools](https://img.shields.io/badge/%C3%89cole%20Polytechnique%20%2F%20HEC%20Paris-2026-black.svg)](https://www.hec.edu/)

> **Computational Foundations Lab** developed as part of the *Python for Data Science* curriculum in the **MSc Data Science & AI for Business** (École Polytechnique / HEC Paris).

---

## 📌 Executive Summary

Writing scalable, production-grade Data Science code requires understanding what happens beneath the Python interpreter. This project explores the foundational computational principles of modern scientific computing:

1. **Floating-Point Precision & IEEE 754 Standard:** Unpacking precision limits, binary fraction representation, catastrophic cancellation, and robust approximate equality testing via `np.allclose()`.
2. **CPython Memory Model & Mutability:** Inspecting memory allocation, object references (`id()`), integer interning, in-place mutations vs. new object rebinding, and shallow vs. deep copy semantics.
3. **Vectorized Computation vs. Native Loops:** Benchmarking the architectural speedups of NumPy contiguous C-arrays (SIMD operations, cache locality, and vectorization) against standard Python `for` loops.
4. **Efficient Python Idioms:** Advanced strided slicing, dictionary hash maps for frequency counting, and memory-efficient list/generator comprehensions.

---

## 🔬 Key Computational Concepts

### 1. IEEE 754 Floating-Point Arithmetic
Computers store real numbers in binary base-2 floating-point representation. Fractions such as $0.1$ cannot be represented exactly in binary (yielding a recurring binary fraction $0.0001100110011..._2$).

```python
result = 0.1 + 100 - 100
print(result == 0.1)          # False! Due to precision rounding
print(np.allclose(result, 0.1)) # True: uses absolute and relative tolerance
```

* **Core Lesson:** Never check exact equality (`==`) on floating-point results in data engineering or financial pipelines; always evaluate within numerical tolerances:
$$|a - b| \le (\text{atol} + \text{rtol} \cdot |b|)$$

---

### 2. CPython Memory Allocation & Object Mutability
Exploring how Python handles pointers and object identity in memory:

```python
a = 1000
b = 1000
print(a is b)      # False (separate memory allocations)

c = a
a += 1             # Integers are immutable: rebinds 'a' to a new integer object
print(c)           # Remains 1000! 'c' still points to the initial memory address
```

* **Data Structure Mechanics:**
  * **Immutable types** (`int`, `float`, `tuple`, `str`): Cannot be modified in-place; operations create new objects.
  * **Mutable types** (`list`, `dict`, `np.ndarray`): In-place modification alters all shared references, a frequent source of silent bugs in data processing pipelines.

---

### 3. Vectorization & SIMD Benchmarking
Comparing array squaring over $N$ elements between Python list comprehensions and contiguous NumPy arrays:

```python
# Native Python loop
squares = [i**2 for i in range(1, 12)]

# NumPy vectorized SIMD computation
np_squares = np.arange(1, 12)**2
```

* **Architectural Differences:**
  * **Native Python:** Interpreted dynamic type checking on each iteration, object boxing/unboxing overhead, and fragmented pointer-based memory layouts causing CPU cache misses.
  * **NumPy:** Contiguous memory buffers, static C data types, SIMD (Single Instruction, Multiple Data) parallel CPU vectorization, yielding **50x–100x speedups** on numerical operations.

---

## 📁 Repository Structure

```text
├── python_data_science_foundations.ipynb  # Complete annotated Jupyter notebook
├── report.html                           # Standalone interactive HTML report
└── README.md
```

---

## 🛠️ Reproduction & Setup

```bash
# Clone the repository
git clone https://github.com/victorpiana/python-data-science-foundations.git
cd python-data-science-foundations

# Environment setup
python -m venv venv
source venv/bin/activate
pip install numpy pandas matplotlib scikit-learn
```

Open notebook:
```bash
jupyter lab python_data_science_foundations.ipynb
```
Or open `report.html` directly in your web browser.

---

## 👨‍💻 Author

**Victor Piana**
* Master of Science in Data Science & AI for Business — **École Polytechnique & HEC Paris**
* LinkedIn: [linkedin.com/in/victor-piana-107230238](https://www.linkedin.com/in/victor-piana-107230238/)
* GitHub: [@victorpiana](https://github.com/victorpiana)
