# Edici-n-de-cadenas-de-caracteres---Equipo-9
Integrantes: González Vargas Nayeli, Ochoa Leos Andrea Guadalupe
from tkinter import *
from tkinter import filedialog, font, messagebox
import pandas as pd

# Variables globales
ruta_archivo = ''
contenido_csv = ''
cadena_original = ''
cadena_modificada = ''
variantes = []

def procesar_csv():
    global cadena_original, cadena_modificada, variantes

    try:
        # Leer el archivo CSV y guardar los datos en un DataFrame
        df = pd.read_csv(ruta_archivo)

        # Guardar la cadena original
        cadena_original = df['string_a_modificar'].iloc[0]

        # Aquí se generan las ventanas de 10 caracteres recorriendo 5 espacios
        for i in range(0, len(cadena_original)-9, 5):
            ventana = cadena_original[i:i+10]
            variantes.append(ventana)

        # Guardar los cambios del dataframe y los convierte en una lista de diccionarios
        cambios = df[['posicion', 'alteracion']].to_dict('records')

        # Se realizan las alteraciones en la cadena original
        cadena_modificada = alteraciones(cadena_original, cambios)

        # Mostrar la cadena original
        txt_output.delete(1.0, END)  # Limpiar la caja de texto antes de mostrar nuevos datos
        txt_output.insert(END, "Cadena original:\n" + cadena_original + "\n\n")

    except Exception as e:
        # Manejar cualquier error inesperado
        messagebox.showerror("Error", f"Se produjo un error: {e}")

def leer_csv():
    global ruta_archivo, contenido_csv
    ruta_archivo = filedialog.askopenfilename(filetypes=[("Archivos CSV", "*.csv")])
    if ruta_archivo:
        try:
            # Leer el contenido del archivo CSV
            df = pd.read_csv(ruta_archivo)
            # Convertir el DataFrame a una cadena y mostrarlo en el área de texto
            contenido_csv = df.to_string(index=False)
            txt_output.delete(1.0, END)
            txt_output.insert(END, contenido_csv)
            # Llamar a la función procesar_csv después de mostrar el contenido
            procesar_csv()
        except Exception as e:
            messagebox.showerror("Error", f"No se pudo abrir el archivo: {e}")

def mostrar_original():
    global cadena_original
    txt_output.delete(1.0, END)
    txt_output.insert(END, "Cadena original:\n" + cadena_original + "\n\n")

def mostrar_modificada():
    global cadena_modificada
    txt_output.delete(1.0, END)
    txt_output.insert(END, "Cadena modificada:\n" + cadena_modificada + "\n\n")

def mostrar_variantes():
    global variantes
    txt_output.delete(1.0, END)
    txt_output.insert(END, "Variantes:\n" + "\n".join(variantes) + "\n\n")

def alteraciones(cadena, cambios):
    nueva_cadena = list(cadena)
    for cambio in cambios:
        posicion = cambio['posicion']
        alteracion = cambio['alteracion']
        if 0 <= posicion < len(nueva_cadena):
            nueva_cadena[posicion] = alteracion
    return ''.join(nueva_cadena)

def generar_combinaciones():
    try:
        inicio = int(entrada_inicio.get())
        fin = int(entrada_fin.get())
        
        if inicio < 1 or inicio > len(variantes) or fin < inicio or fin > len(variantes):
            messagebox.showerror("Error", "Rango de variantes inválido.")
            return
        
        combinaciones = []
        for i in range(inicio - 1, fin):
            for j in range(i + 1, fin + 1):
                combinacion = variantes[i:j]
                combinaciones.append(combinacion)

        txt_output.delete(1.0, END)
        txt_output.insert(END, f"Combinaciones del rango {inicio} al {fin}:\n")
        for combinacion in combinaciones:
            txt_output.insert(END, f"{combinacion}\n")
    except ValueError:
        messagebox.showerror("Error", "Ingrese números válidos para el rango.")

# Configuración de la interfaz
ventana = Tk()
ventana.title("ACTIVIDAD 4")
ventana.geometry("1200x700")
ventana.configure(bg="#F9EBEA")

font_style = font.Font(family="Century Gothic", size=12)

lbl = Label(ventana, text="EQUIPO 9", bg="#F9EBEA", font=("Palatino", 23, "bold"))
lbl.pack(pady=10)

lbl = Label(ventana, text="ACTIVIDAD 4", bg="#F9EBEA", font=("Palatino", 12))
lbl.pack(pady=10)

btn_leer = Button(ventana, text="Seleccionar archivo CSV", command=leer_csv, bg="#FF69B4", fg="white", font=font_style)
btn_leer.pack(pady=5)

btn_original = Button(ventana, text="Mostrar cadena original", command=mostrar_original, bg="#FF69B4", fg="white", font=font_style)
btn_original.pack(pady=5)

btn_modificada = Button(ventana, text="Mostrar cadena modificada", command=mostrar_modificada, bg="#FF69B4", fg="white", font=font_style)
btn_modificada.pack(pady=5)

btn_variantes = Button(ventana, text="Mostrar variantes", command=mostrar_variantes, bg="#FF69B4", fg="white", font=font_style)
btn_variantes.pack(pady=5)

lbl_rango = Label(ventana, text="Ingrese el rango de variantes (inicio y fin del rango):", bg="#F9EBEA", font=font_style)
lbl_rango.pack(pady=5)

frame_rango = Frame(ventana, bg="#F9EBEA")
frame_rango.pack(pady=5)

entrada_inicio = Entry(frame_rango, bg="#FFC0CB", font=font_style)
entrada_inicio.grid(row=0, column=0, padx=5)

entrada_fin = Entry(frame_rango, bg="#FFC0CB", font=font_style)
entrada_fin.grid(row=0, column=1, padx=5)

btn_combinaciones = Button(ventana, text="Generar combinaciones", command=generar_combinaciones, bg="#FF69B4", fg="white", font=font_style)
btn_combinaciones.pack(pady=5)

txt_output = Text(ventana, bg="#F0F0F0", font=("Century Gothic", 12), wrap=WORD)
txt_output.pack(fill=BOTH, expand=True)

ventana.mainloop()
