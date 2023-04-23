# proba
import numpy as np
import matplotlib.pyplot as plt
# функция для генерации случайной матрицы
def generate_random_matrix(n):
    return np.random.normal(0, 1, size=(n, n))
# функция для вычисления числа обусловленности
def cond(A):
    return np.linalg.norm(A, 1) * np.linalg.norm(np.linalg.inv(A), 1)
# порядок матриц
n_values = range(1, 101)
# массивы для хранения значений cond(A_n)
cond_values = []
# генерация случайных матриц и вычисление cond(A_n)
for n in n_values:
    A = generate_random_matrix(n)
    cond_values.append(cond(A))
# построение графика
plt.loglog(n_values, cond_values, 'o', label='cond(A_n)')
# подбор коэффициентов для функций c1*n^p и c2*n^p
p = np.polyfit(np.log(n_values), np.log(cond_values), 1)[0]
c1 = np.exp(np.polyfit(np.log(n_values), np.log(cond_values/n_values**p), 1)[1])
c2 = np.exp(np.polyfit(np.log(n_values), np.log(cond_values/n_values**p), 1)[0])
# построение графиков функций c1*n^p и c2*n^p
plt.loglog(n_values, c1*n_values**p, label=f'{c1:.2f}*n^{p:.2f}')
plt.loglog(n_values, c2*n_values**p, label=f'{c2:.2f}*n^{p:.2f}')
plt.legend()
plt.show()
