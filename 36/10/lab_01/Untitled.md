---
jupytext:
  formats: ipynb,md:myst
  text_representation:
    extension: .md
    format_name: myst
    format_version: 0.13
    jupytext_version: 1.17.3
kernelspec:
  display_name: Python 3 (ipykernel)
  language: python
  name: python3
---

```{code-cell} ipython3
---
editable: true
slideshow:
  slide_type: ''
---
import random
import matplotlib.pyplot as plt
from usage_time import get_usage_time

def sum_func(v):
    """Сумма элементов вектора"""
    s = 0
    for i in range(len(v)):
        s += v[i]
    return s

def mult_func(v):
    """Произведение элементов вектора"""
    m = 1
    for i in range(len(v)):
        m = m * v[i]
    return m

def horner_func(v, x=2):
    """Вычисление полинома методом Горнера"""
    p = 0
    for i in range(len(v)-1, -1, -1):
        p = p * x + v[i]
    return p

time_sum = get_usage_time(number=5, ndigits=6)(sum_func)
time_mult = get_usage_time(number=5, ndigits=6)(mult_func)
time_horner = get_usage_time(number=5, ndigits=6)(horner_func)

sizes = list(range(1, 101000, 1000))

sum_times = []
mult_times = []
horner_times = []

for n in sizes:
    vector = [random.randint(1, 100) for _ in range(n)]

    sum_time = time_sum(vector)
    mult_time = time_mult(vector)
    horner_time = time_horner(vector, 2)
    
    sum_times.append(sum_time)
    mult_times.append(mult_time)
    horner_times.append(horner_time)

plt.figure(figsize=(14, 10))

plt.subplot(2, 1, 1)
plt.plot(sizes, sum_times, 'bo-', label='Sum', linewidth=2, markersize=4, alpha=0.8)
plt.plot(sizes, mult_times, 'ro-', label='Multiplication', linewidth=2, markersize=4, alpha=0.8)
plt.plot(sizes, horner_times, 'go-', label='Horner', linewidth=2, markersize=4, alpha=0.8)

plt.title('Зависимость времени выполнения функций от размера данных (линейный масштаб)')
plt.xlabel('Размер данных (n)')
plt.ylabel('Время выполнения (секунды)')
plt.grid(True, alpha=0.3)
plt.legend()
plt.xticks(sizes[::2], rotation=45)



plt.tight_layout()
plt.show()
```

```{code-cell} ipython3
import random
from usage_time import get_usage_time
import matplotlib.pyplot as plt

# Умножение матриц
def Matrix(n):
    A = [[random.randint(1, 10) for _ in range(n)] for _ in range(n)]
    B = [[random.randint(1, 10) for _ in range(n)] for _ in range(n)]
    C = [[0 for _ in range(n)] for _ in range(n)]
    
    for i in range(n):
        for j in range(n):
            for k in range(n):
                C[i][j] += A[i][k] * B[k][j]
                
    return C

matrix_mult_time = get_usage_time(number=5, ndigits=6)(Matrix)

sizes = list(range(10, 401, 10))
times = []

for n in sizes:
    avg_time = matrix_mult_time(n)
    times.append(avg_time)

plt.figure(figsize=(12, 7))
plt.plot(sizes, times, 'bo-', label='Mult', linewidth=2, markersize=4, alpha=0.8)
plt.xlabel('Размер матрицы (n x n)', fontsize=12)
plt.ylabel('Среднее время выполнения (секунды)', fontsize=12)
plt.title('Время матричного умножения', fontsize=14)
plt.grid(True, alpha=0.3)
plt.yscale('log')
plt.tight_layout()
plt.show()
```

```{code-cell} ipython3

```
