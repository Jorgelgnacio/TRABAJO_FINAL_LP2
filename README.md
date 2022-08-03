# TRABAJO FINAL LP2

Comparación Automatizada de precios de productos de primera necesidad en los 3 Supermercados más populares: Metro, Plaza Vea, Tottus.

## I. DESCRIPCIÓN DEL CASO
Se quiere automatizar la comparación de precios de productos básicos de la canasta familiar (arroz, azúcar, aceite, leche, etc) de tres supermercados que ofrecen sus productos a través de su propio portal web. Estos supermercados son: Metro, Plaza Vea y Tottus. Por lo que se pide escribir un programa en Python utilizando Jupyter notebook que recopile los precios de un producto de la canasta básica familiar de 3 Supermercados anteriormente mencionados y reporte todos sus artículos relacionados con la palabra clave ingresada en la búsqueda, para que posteriormente pueda comparar los precios en los distintos Supermercados de los productos exactamente iguales. Además, que se envíe automáticamente al correo del usuario-consultor un reporte de precio más alto, precio más bajo, productos en común comparando los precios entre los supermercados coincidentes.

## II. INDICACIONES
- El trabajo es grupal con un número mínimo de integrantes 2 y máximo 3.
- Deben crear un repositorio en GitHub que registre el trabajo colaborativo. Es importante no solo colgar el .ipynb sino documentación extra que muestre el planeamiento del desarrollo del proyecto, o cualquier otro que permita evidenciar el detalle del desarrollo del trabajo.
- La url de su respositorio GitHub de trabajo deben ser compartida en el aula virtual en la tarea correspondiente.

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
