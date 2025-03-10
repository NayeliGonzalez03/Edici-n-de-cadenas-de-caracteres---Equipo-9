# Edici-n-de-cadenas-de-caracteres---Equipo-9
Integrantes: González Vargas Nayeli, Ochoa Leos Andrea Guadalupe
import pandas as pd

# Ubicación del archivo CSV
ubi_csv = 'C:\\Users\\dell\\OneDrive\\Documentos\\Actividad_4\\dataset.csv'

# Función para aplicar las alteraciones en la cadena de caracteres
def alteraciones(cadena, cambios):
    nueva_cadena = list(cadena)
    for cambio in cambios:
        posicion = cambio['posicion']
        # Sirve para saber qué cambio se hará en la posición que indica el csv
        alteracion = cambio['alteracion']
        # Verifica si la posición está dentro de los límites de la cadena
        if 0 <= posicion < len(nueva_cadena):
            nueva_cadena[posicion] = alteracion
    # Regresa la cadena modificada
    return ''.join(nueva_cadena)

# Leer el archivo CSV y guardar los datos en un dataframe
df = pd.read_csv(ubi_csv)

# Guardar la cadena que se encuentra en el archivo csv
cadena_original = df['string_a_modificar'].iloc[0]

# Mostrar la cadena original
print("Cadena original:")
print(cadena_original)

# Aquí se generan las ventanas de 10 caracteres recorriendo 5 espacios
ventanas = []
for i in range(0, len(cadena_original)-9, 5):
    ventana = cadena_original[i:i+10]
    ventanas.append(ventana)

# Guardar los cambios del dataframe y los convierte en una lista de diccionarios
cambios = df[['posicion', 'alteracion']].to_dict('records')

# Se realizan las alteraciones en la cadena original
cadena_modificada = alteraciones(cadena_original, cambios)

# Mostrar la cadena modificada con el algoritmo de fuerza bruta
print("\nCadena modificada con el algoritmo de fuerza bruta:")
print(cadena_modificada)

# Función para generar combinaciones por rango
def comb_total(variantes, inicio, fin):
    combinaciones = []
    for i in range(inicio, fin):
        for j in range(i + 1, fin + 1):
            combinacion = variantes[i:j]
            combinaciones.append(combinacion)
    return combinaciones

# Función para mostrar las combinaciones solicitadas por el usuario
def mostrar_combinaciones(variantes):
    inicio_rango = 0
    num_variantes = len(variantes)
    while True:
        try:
            # Solicitar al usuario el rango de combinaciones
            inicio = int(input(f"Ingrese el inicio del rango (1-{num_variantes}): "))
            fin = int(input(f"Ingrese el fin del rango ({inicio}-{num_variantes}): "))
            
            # Validar la entrada del usuario
            if inicio < 1 or inicio > num_variantes or fin < inicio or fin > num_variantes:
                print("¡Entrada inválida! Inténtelo de nuevo.")
                continue
            if fin - inicio < 1:
                print("El rango debe incluir al menos dos variantes. Inténtelo de nuevo.")
                continue
        except ValueError:
            print("¡Entrada inválida! Inténtelo de nuevo.")
            continue
        
        # Generar las combinaciones para el rango dado
        combinaciones = comb_total(variantes, inicio - 1, fin)
        
        # Mostrar las combinaciones
        print(f"\nCombinaciones del rango {inicio} al {fin}:")
        for combinacion in combinaciones:
            print(combinacion)
        
        # Preguntar al usuario si desea continuar
        continuar = input("¿Desea ver otro rango de combinaciones? (Si=1/No=0): ")
        if continuar.lower() != '1':
            break

# Llamar a la función para mostrar las combinaciones solicitadas por el usuario
mostrar_combinaciones(ventanas)

# Convertir las variantes a un DataFrame
df_variantes = pd.DataFrame({'variantes generadas': ventanas})

# Especificar la ubicación del archivo CSV donde se guardarán las variantes
archivo_variantes = 'variantes.csv'

# Guardar el DataFrame en un archivo CSV
df_variantes.to_csv(archivo_variantes, index=False)

print(f'Se guardaron las variantes en el archivo CSV: {archivo_variantes}')
