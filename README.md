# Threat Detectors Challenge

Como equipo de **seguridad informática** tenemos la necesidad de administrar y gestionar las alertas de seguridad de todo el entorno de Mercado Libre.
Por lo cual, necesitamos realizar una API que reciba los eventos generados por una herramienta que detecta actividad maliciosa de las computadoras internas.
Se puede implementar con el lenguaje de programación que prefieras, frameworks y librerias que creas necesarios pero si es importante usar [Docker](https://www.docker.com/) para que sea reproducible facilmente y podamos probarlo.

## REQUISITOS

El challenge consta de una implementación básica obligatoria, dos optativas (**se recomienda implementar al menos una de las dos**) y algunos extras (no obligatorios). La implementación debe contar con al menos un **test unitario** (ya sea de un endpoint o alguna funcionalidad).


## Implementación básica

La API deberá contar con tres métodos (Endpoints) que deben cumplir con este contrato

### Recibir alerta
Metodo: `POST` \
Path: `/alert` \
Body de ejemplo:
```
    {
		"source": "firewall",
		"ioc": {
			"type": "url",
			"data": "https://badurl.evil"
		},
		"user": "pepito",
		"date": "12/07/2022"
	}
```

Se debe guardar la información del JSON para futuras consultas, la forma de almacenarlo queda a criterio propio.
Nota: Los tipos de iocs pueden ser: ip, hash (MD5), url, domain.

Response Code:
- `201` en caso de éxito.
- En caso de error un status code correspondiente al tipo de error, debe de regresar un error si la alerta es repetida.

Response Body:
```
    {
	"id": <id de la alerta>
	"status": "Alerta recibida correctamente",
    }
```

Curl de ejemplo:
```
curl --request POST \
      --url http://localhost:8080/alert \
      --header 'content-type: application/json' \
      --data '{
		"source": "firewall",
		"ioc": {
			"type": "url",
			"data": "https://badurl.evil"
		},
		"user": "pepito",
		"date": "12/07/2022"
	  }'
```

### Leer las alertas creadas por un usuario, en los últimos N días
Descripción: Se debe poder consultar las alertas generadas por un usuario en específico de los últimos N días.

Metodo: `GET` \
Path: `/alerts?user=<user>&days=<days>`

Response Code:
- `200` en caso de éxito.
- `204` en caso de no encontrar nada.
- En caso de error un status code correspondiente al tipo de error.

Response Body de ejemplo:
```
    {
		"alerts": [
			{
				"source": "firewall",
				"ioc": {
					"type": "url",
					"data": "https://badurl.evil"
				},
				"user": "pepito",
				"date": "12/07/2022"
			},
			{
				"source": "ips",
				"ioc": {
					"type": "ip",
					"data": "6.6.6.6"
				},
				"user": "pepito",
				"date": "13/07/2022"
			}
		]
	}
```

Curl de ejemplo:   
```
curl --request GET \
      --url 'http://localhost:8080/alerts?user=<user>&days=<days>'
```

### Leer alertas por tipo de IOC
Descripción: Agregar un parámetro (_Query Parameter_) para consultar las alertas de un tipo específico de IOC de los últimos N días.

Metodo: `GET` \
Path: `/alerts?ioc_type=<ioc_type>&days=<days>`

Response Code:
- `200` en caso de éxito.
- `204` en caso de no encontrar nada.
- En caso de error un status code correspondiente al tipo de error.

Curl de ejemplo:   
```
curl --request GET \
      --url 'http://localhost:8080/alerts?type=ip'
```

## Implementación optativa 1
Sumar a la implementación el chequeo de alertas repetidas (usando el criterio que crea conveniente).

## Implementación optativa 2
Se requiere también utilizar esta app para validar la información de diferentes IOCs, para lo cual se debe realizar un endpoint que consulte en alguna fuente pública (como Virus Total, PhishTank, etc) la reputación de un IOC proporcionado.

## Extras
- Logging
- Documentación
- Autenticación en la api
- Sumar funcionalidades (ejemplo: validar información que reciben los endpoints, implementar async en el endpoint de Consulta a fuentes externas, o cualquiera adicional que se le quiera sumar)

**Recordá que cuanto mas fácil sea de reproducir y podamos ver todo lo que hiciste, mejor :D**
