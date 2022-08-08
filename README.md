# TRABAJO FINAL LP2

- Tanta Medina, Christian Rodolfo 20200338
- Taquiri Pillaca, Jorge Ignacio 20200339
- Tay Saccaco, Brian Eduardo 

# Comparación Automatizada de precios de productos de primera necesidad en los 3 Supermercados más populares: Metro, Plaza Vea, Tottus.

## I. DESCRIPCIÓN DEL CASO
Se quiere automatizar la comparación de precios de productos básicos de la canasta familiar (arroz, azúcar, aceite, leche, etc) de tres supermercados que ofrecen sus productos a través de su propio portal web. Estos supermercados son: Metro, Plaza Vea y Tottus. Por lo que se pide escribir un programa en Python utilizando Jupyter notebook que recopile los precios de un producto de la canasta básica familiar de 3 Supermercados anteriormente mencionados y reporte todos sus artículos relacionados con la palabra clave ingresada en la búsqueda, para que posteriormente pueda comparar los precios en los distintos Supermercados de los productos exactamente iguales. Además, que se envíe automáticamente al correo del usuario-consultor un reporte de precio más alto, precio más bajo, productos en común comparando los precios entre los supermercados coincidentes.

## II. INDICACIONES
- El trabajo es grupal con un número mínimo de integrantes 2 y máximo 3.
- Deben crear un repositorio en GitHub que registre el trabajo colaborativo. Es importante no solo colgar el .ipynb sino documentación extra que muestre el planeamiento del desarrollo del proyecto, o cualquier otro que permita evidenciar el detalle del desarrollo del trabajo.
- La url de su respositorio GitHub de trabajo deben ser compartida en el aula virtual en la tarea correspondiente.

# Importar librerías

```{python}
import pandas as pd
import time
from selenium import webdriver
from selenium.webdriver.chrome.service import Service
from selenium.webdriver.common.by import By
from selenium.webdriver.common.keys import Keys
from selenium.webdriver.support import expected_conditions as EC
from selenium.webdriver.support.ui import WebDriverWait
from pymongo import MongoClient
from tqdm import tqdm
from termcolor import colored
import csv
from bs4 import BeautifulSoup
from selenium import webdriver
import time
#jupyter notebook --NotebookApp.iopub_data_rate_limit=1.0e10
```

# Documentación de la nueva actualización de Selenium

```{python}
ID = "id"
NAME = "name"
XPATH = "xpath"
LINK_TEXT = "link text"
PARTIAL_LINK_TEXT = "partial link text"
TAG_NAME = "tag name"
CLASS_NAME = "class name"
CSS_SELECTOR = "css selector"

# find_element(By.ID, "id")
# find_element(By.NAME, "name")
# find_element(By.XPATH, "xpath")
# find_element(By.LINK_TEXT, "link text")
# find_element(By.PARTIAL_LINK_TEXT, "partial link text")
# find_element(By.TAG_NAME, "tag name")
# find_element(By.CLASS_NAME, "class name")
# find_element(By.CSS_SELECTOR, "css selector")
```

# Programa para obtener los productos y precios de supermercados Metro (Usuario Jorge)


```{python}
path = ('C:/Users/jorge/Documents/TRABAJO FINAL/chromedriver.exe')  # Dirección del ejecutable chromedriver.exe
urls='https://www.metro.pe'          # URL de la pagina de Metro                   
options = webdriver.ChromeOptions()   
options.add_argument('--incognito')  # Opcion de abrir ventana de Chrome en modo incognito
s = Service(path)                       
driver = webdriver.Chrome(service=s, options = options)
driver.maximize_window()
driver.get(urls)

time.sleep(3) # lapso de 3 segundo para que la pagina cargue los contenidos
```
```{python}
# Hacer clic en "cerrar", pagina emergente que sale en inicio
driver.find_e```{python}lement(By.XPATH, '/html/body/div[46]/div/div[3]/button[1]').click()
time.sleep(1)
```

```{python}
# Hacer clic en la barra de búsqueda
driver.find_element(By.XPATH, '/html/body/header[1]/div/div[2]/div[2]/div[1]/div[1]/input').click()
time.sleep(1)
```

```{python}
# Ingresar el producto en la barra de busqueda
driver.find_element(By.XPATH, '/html/body/header[1]/div/div[2]/div[2]/div[1]/div[1]/input').send_keys('leche')
time.sleep(1)
```

```{python}
# Dar clic en ver todos los resultados.
driver.find_element(By.XPATH, '/html/body/header[1]/div/div[2]/div[2]/div[1]/div[2]/div[2]/a/span').click()
time.sleep(1)
```

```{python}
# Cantidad de productos
page=driver.find_element(By.XPATH, '/html/body/div[27]/div/div[2]/div[1]/div[1]/span[2]').text
int(page[:3])
```

```{python}
# Automatización de scrolls (se correra las veces que sea necesario)
tiempo = 15 + 1           # Seran 15 scrolls automaticos
while True:               # Mientras esto sea verdad se realizará lo siguiente:
    tiempo -= 1           # Cuenta regresiva, se resta 1 cada ciclo
    driver.execute_script("window.scrollTo(0, document.body.scrollHeight)") # Cada segundo se desplazará hasta la parte inferior de la página 
    time.sleep(1)      # Cada scroll automático tendra un timelap de 1 segundo
    if scrolls < 0:     # Cuando llegue a cero segundos:
        time.sleep(3)   # Le damos 3 segundos a que la pagina carge los contenidos hasta el ultimo scroll automatico
        driver.execute_script("window.scrollBy(0,-310)") # Un scroll up (rueda hacia arriba) para mostrar la ultima fila de productos
        elements = driver.find_elements(By.XPATH,'//li[@layout="19ccd66b-b568-43cb-a106-b52f9796f5cd"]') # Encontrar los productos de la pagina de acuerdo al XPATH
        break           # Se cierra el ciclo

        
print(colored('Productos extraidos:','red'))  
print(len(elements))        # Longitud de los elementos (productos) encontrados hasta coindicir con el total de productos

print(colored('\n Se llegó a extraer todos los productos?.','yellow'))  
print(len(elements) == int(page[:3])) # Comparación 
```

```{python}
# Vista a todos los elementos a scrapear, desde el primero hasta el ultimo en ser extradio
n = 1
while n < len(elements):  # Ciclo de acuerdo  a la cantidad de elementos extraidos
    listing = driver.find_element(By.XPATH, f'/html/body/div[27]/div/div[2]/div[4]/div[2]/div[2]/div[2]/div/ul/li[{n}]') 
    driver.execute_script("arguments[0].scrollIntoView(true);", listing)  # Se ira a la parte inferior hasta la ultima fila de productos
    n += 1
```

```{python}
# Almacenar productos y precios 
productos =  []  # lista vacia
precios = []  # lista vacia
for element in tqdm(elements):  # tqdm:Contar iteraciones, indicar el tiempo que tarda por iteración y el tiempo total transcurrido.  
        name = element.find_element(By.XPATH,'.//a[@class="product-item__name"]').text  # Buscar producto en base al XPATH y mostrar el texto
        price = element.find_element(By.XPATH,'.//span[@class="product-prices__value product-prices__value--best-price"]').text # Buscar precio en base al XPATH y mostrar el texto
        productos.append(name)    # Agregar a la lista productos
        precios.append(price)  # Agregar a la lista precios
```

```{python}
#Exportacion de productos y precios a un archivo excel 
dic= {'Producto': productos , "Precio": precios}  # Crear diccionario con los values productos y precios (listas)
df = pd.DataFrame(dic , columns= ["Producto", "Precio"] ) # Convertir a dataframe con columnas de etiquetas Producto y Precio
df.to_excel('metro.xlsx', index=False,encoding='cp1252')  # Exportar a excel, sin indices, y el encoding cp1252

# Lectura y muestra de resultados
print(colored('Imprimiendo resultados.','blue'))  
pd.read_excel('metro.xlsx')
```

# Programa para obtener los productos y precios de supermercados Tottus  (Usuario Christian)



