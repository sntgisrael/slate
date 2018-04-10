---
title: API Reference

language_tabs: # must be one of https://git.io/vQNgJ
  - JSON


toc_footers:
  - <a href='#'>Sign Up for a Developer Key</a>
  - <a href='https://github.com/lord/slate'>Documentation Powered by Slate</a>

includes:
  - errors

search: true
---

# Introducción

Bienvenido a la API de Rondanet.

Mediante la API podrá emitir registrar y editar productos como también consultar los registrados.

A la derecha se muestran desplegados los ejemplos en formato Json.

## Ambientes

<!-- Existen dos ambientes para operar en Biller: testing y producción. Estos se encuentran completamente separados, por lo que la creación de una cuenta de usuario en un ambiente no implica la disponibilidad de la misma en el otro.

Cada ambiente utiliza su ambiente homólogo en DGI por lo que cualquier comprobante que sea emitido en Biller será a su vez enviado al correspondiente en DGI.

El ambiente que se desea utilizar debe indicarse en la URL de la API a utilizar. -->

## Versionado
<!-- A la fecha, la API de Biller se encuentra en su versión 1. 

Distintos cambios de funcionamiento internamente son introducidos constantemente pero ninguno de ellos debería afectar los desarrollos de nuestros usuarios. Sin embargo, se introducirán cambios en el futuro que afectarán la backwards compatibility, por lo que se utilizarán API endpoints distintos para las distintas versiones.

La versión de la API a utilizar debe ser especificada en la URL invocada. -->

## URL
<!-- La URL diseñada para interactuar con Biller tiene la forma:

`https://{ambiente.}biller.uy/{versión}/{llamado}?{parámetros}`

### Ejemplos

POST `https://test.biller.uy/v1/comprobantes/crear`

GET `https://www.biller.uy/v1/comprobantes/pdf?id=2000` -->

# Autenticación

<!-- Actualmente todos los llamados requieren de autenticación mediante Bearer Token.

Cada token está asociado unívocamente a una empresa, por lo que será necesario primero crear un usuario en el sistema, luego crear una empresa y finalmente generar su token.

La URL para generación de tokens en testing es: 

`https://test.biller.uy/api-tokens` 

y en producción:

 `https://www.biller.uy/api-tokens`

Para poder utilizarlo es necesario agregar los siguientes encabezados al paquete HTTP:

`Authorization: Bearer su-token-aqui `

`Content-Type: application/json` -->

# Llamados

## producto

Se registra un producto a partir de los parámetros enviados.

> Registro de un producto:

```json
{
	"glnPublicador": 1023,
    "cpp": 12132552,
    "gtin": 214741128,
    "descripcion": "TRESEMME CR TRAT CONT CAIDA 400G",
    "marca": "TRESEMME",
    "categoriaPrincipal": "PERSONAL CARE",
    "categoriaSecundaria": "Cremas de Peinar o Tratamiento",
    "categoriaGpc": null,
    "presentacion": null,
    "contenidoNeto": 400,
    "unidadMedida": "gr",
    "paisOrigen": "AR",
    "foto": null,
    "alto": 10,
    "ancho": 10,
    "profundidad": 10,
    "pesoBruto": 10,
    "idInferior": [1,2,3],
    "tipoIdInferior": [1,1,1],
    "cantidadInferior": [10,10,10],
    "gtinUnidad": [415522],
    "unidadesCamada": 10,
    "camadasPallet": 10,
    "posicion": 1,
    "visiblePor": [1,2],
    "pediblePor": [1,2]
}
```


> Se obtiene la respuesta:

```json
{
    "code": 200,
    "data": 8
}
```



### HTTP Request

`POST /producto`

### Entrada

Clave | Valor | Detalle
--------- | ---------- | ---------------------
glnPublicador | long | 
cpp | long | Código Producto Proveedor de la unidad mínima de venta (EA)
gtin | long | Código de identifcación del producto GS1
descripcion | string | Descripción comprensible y utilizable del artículo
marca | string | Nombre comercial del fabricante del producto
categoriaPrincipal | string | Clasificación divisional definida por el Proveedor(Ej. Cuidado Personal y del Hogar)
categoriaSecundaria | string | Clasificación por Familia definida por el Proveedor (Ej. Limpiadores líquidos)
categoria Gpc | string | Clasificación estándar de productos (Global product clasification)
presentacion | string | Tipo de Packaging del producto (Ej. Caja, sachet, funda)
contenidoNeto | long | Cantidad real del producto comercializado
unidadMedida | string | Unidad en que se mide el contenido neto del producto, Ej. Ltr, Kgr.
paisOrigen | string | Se considera el país de origen en donde se envasa el producto final independientemente del país de procedencia de la materia prima.
foto | string |
alto | long | Altura del producto referente a su posición en góndola o en estantería
ancho | long | Ancho del producto referente a su posición en góndola o en estantería
profundidad | long | Profundidad del producto referente a su posición en góndola o en estantería
pesoBruto | long | Peso neto del producto  mas el  peso del packaging de la unidad de venta
idInferior | array | Código del artículo contenido inmediato
tipoIdInferior | array |
cantidadInferior | array | 
gtinUnidadMin | long | Código de identifcación de la mínima unidad de venta
unidadesCamada | long | 
camadasPallet | long |
posicion | long | Posición en la que el producto se visualiza en Pedidos
visiblePor | array |
pediblePor | array | 

