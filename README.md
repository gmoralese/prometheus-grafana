## Compose sample
### Prometheus & Grafana

Estructura del proyecto:
```
.
├── compose.yaml
├── grafana
│   └── datasource.yml
├── prometheus
│   └── prometheus.yml
└── README.md
```

[_compose.yaml_](compose.yaml)
```
services:
  prometheus:
    image: prom/prometheus
    ...
    ports:
      - 9090:9090
  grafana:
    image: grafana/grafana
    ...
    ports:
      - 3000:3000
```

El archivo compose define una pila con dos servicios `prometheus` y `grafana`. Al desplegar la pila, docker compose mapea los puertos predeterminados de cada servicio a los puertos equivalentes en el host para facilitar la inspección de la interfaz web de cada servicio. Asegúrate de que los puertos 9090 y 3000 en el host no estén ya en uso.

## Despliegue con docker compose

```
$ docker compose up -d
Creating network "prometheus-grafana_default" with the default driver
Creating volume "prometheus-grafana_prom_data" with default driver
...
Creating grafana    ... done
Creating prometheus ... done
Attaching to prometheus, grafana

```

## Resultado esperado

La lista de contenedores debe mostrar dos contenedores en ejecución y el mapeo de puertos como a continuación:
```
$ docker ps
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                    NAMES
dbdec637814f        prom/prometheus     "/bin/prometheus --c…"   8 minutes ago       Up 8 minutes        0.0.0.0:9090->9090/tcp   prometheus
79f667cb7dc2        grafana/grafana     "/run.sh"                8 minutes ago       Up 8 minutes        0.0.0.0:3000->3000/tcp   grafana
```

Navega a http://localhost:3000 en tu navegador web y usa las credenciales de inicio de sesión especificadas en el archivo de composición para acceder a Grafana. Ya está configurado con prometheus como origen de datos por defecto.

![page](output.jpg)

Navega a `http://localhost:9090` en tu navegador web para acceder directamente a la interfaz web de prometheus.

Detén y elimina los contenedores. Usa `-v` para eliminar los volúmenes si deseas borrar todos los datos.

```
$ docker compose down -v
```
