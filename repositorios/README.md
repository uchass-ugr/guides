# Cómo organizar el repositorio de un proyecto o un paper

Esta guía explica cómo montar un repositorio en GitHub para que cualquiera, incluido tú dentro de dos años, entienda qué hay, pueda reproducir los resultados y sepa qué se puede compartir y qué no.

Son recomendaciones. Quién crea los repositorios, quién tiene acceso y dónde se guardan los datos que no se pueden compartir lo decide el IP de cada proyecto.

## 1. Un repositorio por proyecto y uno por paper

- **Repositorio de proyecto** (`<proyecto>`): web, documentación y código común a varios papers.
- **Repositorio de paper** (`<proyecto>-<paper>`): el código y los resultados de un solo trabajo. Por ejemplo, `stitch-credit-geo`.

Separarlos tiene tres ventajas:

- **Visibilidad.** Un repositorio es entero público o entero privado. El proyecto puede ser público mientras un paper en curso sigue privado.
- **DOI.** Zenodo asigna un DOI por repositorio. Cada paper tiene así su propio paquete de replicación citable.
- **Autoría.** Cada paper tiene sus autores. El historial y la cita quedan limpios.

Usa nombres en minúsculas, cortos y con guiones: `project-paper`, no `Paper_CRediT_final_v2`.

## 2. Estructura de carpetas

Parte de la [plantilla](../plantillas/repositorio/) y mantén esta estructura:

```
<proyecto>-<paper>/
├── README.md          Qué es, cómo reproducirlo, cómo citarlo
├── LICENSE            Licencia del código
├── LICENSE-data       Licencia de datos abiertos y textos
├── CITATION.cff       Cómo citar el repositorio
├── .gitignore         Qué no se sube nunca
├── code/              Scripts, numerados en orden de ejecución
├── data/
│   ├── open/          Datos que se pueden publicar
│   ├── processed/     Ficheros intermedios (no se suben)
│   └── restricted/    Datos que no se pueden compartir (no se suben)
└── outputs/           Figuras y tablas finales
```

La regla de fondo: **todo lo que hay en `outputs/` debe poder regenerarse ejecutando `code/` sobre `data/`.**

## 3. Datos abiertos y datos restringidos

Antes de empezar, decide en qué categoría está cada fuente de datos.

**Datos abiertos** (OpenAlex, datos propios publicables, agregados que permite el acuerdo):

- Deposítalos en [Zenodo](https://zenodo.org) y enlaza el DOI en el README.
- Si son pequeños, también pueden ir en `data/open/`.
- Documenta qué es cada variable en un codebook (`data/open/README.md` o un fichero aparte).

**Datos restringidos** (bajo licencia, cedidos por un proveedor, con datos personales):

- **No se suben nunca a GitHub, ni siquiera a un repositorio privado.** Un repo privado se puede hacer público por error, y el historial guarda todo lo que se ha subido alguna vez.
- El código los lee desde una ruta definida en un fichero de configuración local que no se sube (`.Renviron` en R, `.env` en Python). Así el código funciona en el ordenador de cada persona sin escribir rutas personales.
- `data/restricted/README.md` explica qué datos son, quién los proporciona, bajo qué acuerdo y a quién hay que pedir acceso.
- Los ficheros derivados (`data/processed/`) heredan las restricciones de los datos de origen.

Si no sabes en qué categoría está algo, trátalo como restringido y pregunta al IP.

## 4. Código

- **Numera los scripts** en el orden en que se ejecutan: `01_extraer.sql`, `02_limpiar.R`, `03_figuras.py`.
- **Usa rutas relativas** a la raíz del repositorio. Nada de `C:\Users\...` ni `/home/...`.
- **Congela el entorno**: deja registradas las versiones de los paquetes que usas (`renv.lock` en R, `requirements.txt` o `environment.yml` en Python).
- **Explica cómo ejecutarlo** en el README: qué hace falta instalar, qué ficheros de datos se necesitan y en qué orden se lanzan los scripts.
- **No subas credenciales.** Contraseñas, claves de API y tokens van en el fichero de configuración local, nunca en el código.

## 5. Licencias

- **Código:** [MIT](https://opensource.org/license/mit).
- **Datos abiertos y textos:** [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/deed.es).

Los datos restringidos no llevan licencia nuestra: se rigen por su acuerdo.

## 6. Citar y agradecer

- Rellena `CITATION.cff` con los autores y el título. GitHub muestra entonces un botón *Cite this repository*.
- Incluye en el README la frase de financiación del proyecto y, si se usan datos cedidos, el agradecimiento que exija el acuerdo.

## 7. Al terminar

Cuando el paper esté publicado y el repositorio sea público, **archívalo** (*Settings → Archive this repository*). Queda en solo lectura, visible y citable, y se puede desarchivar si hace falta.

## Checklist antes de hacer público un repositorio

- [ ] No hay datos restringidos ni derivados de ellos, tampoco en el historial
- [ ] No hay credenciales ni rutas personales en el código
- [ ] El README explica qué es, cómo reproducirlo y cómo citarlo
- [ ] Los datos abiertos tienen DOI o están en `data/open/` con su codebook
- [ ] `LICENSE`, `LICENSE-data` y `CITATION.cff` están completos
- [ ] La frase de financiación y los agradecimientos están en el README
- [ ] Alguien distinto de quien lo escribió ha ejecutado el código desde cero
