import numpy as np
import matplotlib.pyplot as plt
from scipy.optimize import curve_fit

# Datos proporcionados
tiempo = np.array([0, 4, 5, 8, 12, 17, 20, 22, 27, 35, 41, 47, 55, 63, 71, 81, 90, 105, 110, 125, 135, 150, 160, 180, 190, 210, 230, 245, 265, 290, 310, 330, 355, 380, 410, 435, 470, 505, 535, 575, 610, 645, 685, 725, 765, 810, 850, 900, 945, 1000, 1045, 1095, 1150, 1205, 1260, 1320, 1375, 1435, 1500, 1555, 1625, 1690, 1760, 1820, 1890, 1960, 2035, 2105, 2180, 2255, 2330, 2410, 2490, 2570, 2650, 2730, 2815, 2900, 2990, 3080, 3165, 3255, 3345, 3440, 3540, 3630, 3725, 3825, 3930, 4020, 4130, 4225, 4335, 4440, 4545, 4655, 4770, 4875, 4985, 5100, 5215, 5335, 5460])
voltaje = np.array([27.7, 27.553, 27.406, 27.259, 27.112, 26.965, 26.818, 26.671, 26.524, 26.376, 26.229, 26.082, 25.935, 25.788, 25.641, 25.494, 25.347, 25.2, 25.053, 24.906, 24.759, 24.612, 24.465, 24.318, 24.171, 24.024, 23.876, 23.729, 23.582, 23.435, 23.288, 23.141, 22.994, 22.847, 22.7, 22.553, 22.406, 22.259, 22.112, 21.965, 21.818, 21.671, 21.524, 21.376, 21.229, 21.082, 20.935, 20.788, 20.641, 20.494, 20.347, 20.2, 20.053, 19.906, 19.759, 19.612, 19.465, 19.318, 19.171, 19.024, 18.876, 18.729, 18.582, 18.435, 18.288, 18.141, 17.994, 17.847, 17.7, 17.553, 17.406, 17.259, 17.112, 16.965, 16.818, 16.671, 16.524, 16.376, 16.229, 16.082, 15.935, 15.788, 15.641, 15.494, 15.347, 15.2, 15.053, 14.906, 14.759, 14.612, 14.465, 14.318, 14.171, 14.024, 13.876, 13.729, 13.582, 13.435, 13.288, 13.141, 12.994, 12.847, 12.7])

# Función exponencial para el ajuste (modelo empírico)
def exp_func(t, A, B):
    return A * np.exp(B * t)

# Ajustar modelo empírico a los datos
popt, _ = curve_fit(exp_func, tiempo, voltaje, p0=[27.7, -0.0001])
A, B = popt

# Generar puntos para la curva ajustada (modelo Python)
t_fit = np.linspace(0, 15000, 1000)
y_fit = exp_func(t_fit, A, B)

# Modelo teórico con tau conocido
K = 27.7       # Voltaje inicial
tau = 5657     # Constante de tiempo (calculada por ti)
t_teo = np.linspace(0, 15000, 1000)
V_teo = K * np.exp(-t_teo / tau)

# Crear gráfico
plt.figure(figsize=(10, 6))

# Gráfico de datos medidos
plt.scatter(tiempo, voltaje, color='blue', label='Datos', s=50)

# Gráfico del ajuste realizado por Python
plt.plot(t_fit, y_fit, 'r-', label=f'Ajuste Python: {A:.2f}·exp({B:.6f}·t)')

# Gráfico del modelo teórico usando tau
plt.plot(t_teo, V_teo, 'g--', label=f'Teórico: {K:.1f}·exp(-t/{tau:.0f})')

# Ejes y leyenda
plt.xlim(0, 15000)
plt.ylim(0, 30)
plt.xlabel('Tiempo (s)')
plt.ylabel('Voltaje (V)')
plt.title('Descarga de un circuito RC')
plt.legend()
plt.grid(True)
plt.tight_layout()
plt.show()

# Mostrar parámetros ajustados
print(f"Parámetros ajustados por Python:\nA = {A:.2f}, B = {B:.6f}")
