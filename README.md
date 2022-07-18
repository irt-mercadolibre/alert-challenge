# Threat Detectors Challenge

Como equipo de **seguridad informática** tenemos la necesidad de administrar y gestionar las alertas de seguridad de todo el entorno de Mercado Libre.
Por lo cual, necesitamos realizar una API que reciba los eventos generados por una herramienta que detecta actividad maliciosa de las computadoras internas.
Se puede implementar con el lenguaje de programación que prefieras, frameworks y librerias que creas necesarios pero si es importante usar [Docker](https://www.docker.com/) para que sea reproducible facilmente y podamos probarlo.

## REQUISITOS

1. El challenge consta de una implementación básica obligatoria, dos optativas (**se recomienda implementar al menos una de las dos**) y algunos extras (no obligatorios).
2. La implementación debe contar con al menos un **test unitario** (ya sea de un endpoint o alguna funcionalidad).


## Implementación básica

La API deberá contar con dos endpoints que deben cumplir con este contrato.

### Recibir alerta
**Metodo**: `POST` \
**Path**: `/alert` \
**Body de ejemplo**:
```json
{
	"source": "firewall",
	"ioc": {
		"type": "url",
		"data": "https://badurl.evil"
	},
	"user": "pepito",
	"datetime": "2022-07-12 12:00:00"
}
```

Se debe guardar la información del JSON para futuras consultas, la forma de almacenarlo queda a criterio propio.
Nota: Los tipos de iocs pueden ser: ip, hash (MD5, SHA256), url, domain.

**Response Code**:
- `201` en caso de éxito.
- En caso de error, retornar el código de error correspondiente.

**Response Body**:
```json
{
    "id": <id de la alerta>,
    "status": "Alerta recibida correctamente",
}
```

**Curl de ejemplo**:
```bash
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
		"datetime": "2022-07-12 12:00:00"
	  }'
```

### Consultar alertas

**Metodo**: `GET` \
**Path**: `/alerts`

Se debe crear un endpoint para consultar las alertas generadas, en base a los siguientes parámetros de consulta (_query parameters_):

1. Alertas creadas por un usuario.
```bash
curl --request GET \
      --url 'http://localhost:8080/alerts?user=pepito'
```

2. Alertas creadas en los últimos N días.
```bash
curl --request GET \
      --url 'http://localhost:8080/alerts?days=7'
```

3. Alertas que contengan iocs de cierto tipo (ip, hash, domain, url)
```bash
curl --request GET \
      --url 'http://localhost:8080/alerts?ioc_type=domain'
```

**Response Code**:
- `200` en caso de éxito.
- `204` en caso de no encontrar nada.
- En caso de error, retornar el código de error correspondiente.

**Response Body**:
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
				"datetime": "2022-07-12 12:00:00"
			},
			{
				"source": "ips",
				"ioc": {
					"type": "ip",
					"data": "6.6.6.6"
				},
				"user": "pepito",
				"datetime": "2022-07-12 12:00:00"
			}
		]
	}
```

Los _query parameters_ anteriormente especificados pueden ser unificados para realizar una consulta más compleja. Ejemplo:
```
curl --request GET \
      --url 'http://localhost:8080/alerts?ioc_type=domain&days=7&user=pepito'
```

## Implementación optativa 1
Sumar a la implementación el chequeo de alertas repetidas (usando el criterio que crea conveniente).

## Implementación optativa 2
Se requiere también utilizar esta app para validar la información de diferentes IOCs, para lo cual se debe realizar un endpoint que consulte en alguna fuente pública (como Virus Total, PhishTank, etc) la reputación de un IOC proporcionado.

## Extras
- Logging
- Validación de datos entrantes
- Documentación
- Autenticación en la api
- Sumar funcionalidades (ejemplo: validar información que reciben los endpoints, implementar async en el endpoint de Consulta a fuentes externas, agregar _query parameters_ o cualquiera adicional que se le quiera sumar)

**Recordá que cuanto mas fácil sea de reproducir y podamos ver todo lo que hiciste, mejor :D**
