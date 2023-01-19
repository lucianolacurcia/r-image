## Cómo crear entornos de desarrollo custom
Para buildear imagen:

Los entornos de desarrollo son imagenes docker con librerías y runtimes específicos instalados.

Se utiliza una herramienta llamada repo2docker la cual toma como input un repositorio git (o un directorio local), lee archivos en la raíz del repo y construye la imagen instalando lo dispuesto en dichos archivos.

[https://repo2docker.readthedocs.io/en/latest/config_files.html](Aquí) se encuentra la documentacion de cada archivo.

en nuestro caso, para buildear:

```
repo2docker github.com/lucianolacurcia/r-image
```

Esto genera una imagen que queda en el repositorio local de imagenes de docker, luego debemos pushearlo al repo del cluster de openshifta usando `docker push` (quizá cambiarle el nombre y retaguearlo también)

Para poder usar la nueva imagen en jupyterhub, tenemos que modificar la lista de imagenes single-user que aparece en la config del helm chart de jupyterhub, en el archivo valuse.yml, en esta parte:

```
singleuser:
  profileList: # podemos poner una lista de multiples imagenes para que los usuarios eligan
  - display_name: "Rstudio estadística IMM + Python" # Acá el nombre del entorno
    description: "R 4.1.2 con las bibliotecas instaladas." # Acá la descripción
    kubespawner_override:
      image: "lucianolacurcia/jupyterhub-rstudio:94d4b1611e25"
      default_url: "/lab"
```
