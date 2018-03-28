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

Bienvenido a la API de Biller!

Mediante la API podrá emitir comprobantes y obtener los PDFs de los comprobantes emitidos.

A la derecha se muestran desplegados los ejemplos en formato Json.

## Ambientes

Existen dos ambientes para operar en Biller: testing y producción. Estos se encuentran completamente separados, por lo que la creación de una cuenta de usuario en un ambiente no implica la disponibilidad de la misma en el otro.

Cada ambiente utiliza su ambiente homólogo en DGI por lo que cualquier comprobante que sea emitido en Biller será a su vez enviado al correspondiente en DGI.

El ambiente que se desea utilizar debe indicarse en la URL de la API a utilizar.

## Versionado
A la fecha, la API de Biller se encuentra en su versión 1. 

Distintos cambios de funcionamiento internamente son introducidos constantemente pero ninguno de ellos debería afectar los desarrollos de nuestros usuarios. Sin embargo, se introducirán cambios en el futuro que afectarán la backwards compatibility, por lo que se utilizarán API endpoints distintos para las distintas versiones.

La versión de la API a utilizar debe ser especificada en la URL invocada.

## URL
La URL diseñada para interactuar con Biller tiene la forma:

`https://{ambiente.}biller.uy/{versión}/{llamado}?{parámetros}`

### Ejemplos

POST `https://test.biller.uy/v1/comprobantes/crear`

GET `https://www.biller.uy/v1/comprobantes/pdf?id=2000`

# Autenticación

Actualmente todos los llamados requieren de autenticación mediante Bearer Token.

Cada token está asociado unívocamente a una empresa, por lo que será necesario primero crear un usuario en el sistema, luego crear una empresa y finalmente generar su token.

La URL para generación de tokens en testing es: 

`https://test.biller.uy/api-tokens` 

y en producción:

 `https://www.biller.uy/api-tokens`

Para poder utilizarlo es necesario agregar los siguientes encabezados al paquete HTTP:

`Authorization: Bearer su-token-aqui `

`Content-Type: application/json`

# Llamados

## comprobantes/crear

Emite un comprobante a nombre de la empresa propietaria del token, con los datos enviados en el cuerpo de la solicitud.

> Emisión de un eTicket sin receptor:

```json
{
    "tipo_comprobante": 101,
    "forma_pago": 1, 
    "sucursal": 1, 
    "moneda": "UYU",
    "montos_brutos": 1,
    "cliente": "-",
    "items": [
        {
            "cantidad": 1,
            "concepto": "Pelota de fútbol",
            "precio": 200,
            "indicador_facturacion": 3
        }
    ]
}
```

> Emisión de una nota de crédito de eTicket:

```json
{
    "tipo_comprobante": 102,
    "forma_pago": 1, 
    "sucursal": 1, 
    "moneda": "UYU",
    "montos_brutos": 1,
    "cliente": "-",
    "items": [
        {
            "cantidad": 1,
            "concepto": "Anulación de e-Factura A-1",
            "precio": 200,
            "indicador_facturacion": 3
        }
    ],
    "tipo_referencia": "101",
    "serie_referencia": "A",
    "numero_referencia": 1,
}
```

> Emisión de un eTicket con receptor:

```json
{
    "tipo_comprobante": 101,
    "forma_pago": 1, 
    "sucursal": 1, 
    "moneda": "UYU",
    "montos_brutos": 1,
    "cliente": {
        "tipo_documento": 3, 
        "documento": "47395795",
        "nombre_fantasia": "Santiago Israel",
        "sucursal": {
            "pais": "UY"
        }
    },
    "items": [
        {
            "cantidad": 1,
            "concepto": "Pelota de fútbol",
            "precio": 456,
            "indicador_facturacion": 3
        }
    ]
}
```
> Emisión de una eFactura con descuentos/recargos:

```json
{
    "forma_pago": 2,
    "fecha_vencimiento": "31/12/2016",
    "sucursal": 1, 
    "moneda": "UYU",
    "cliente": {
        "tipo_documento": 2, 
        "documento": "214987440015",
        "razon_social": "Arcos Plateados SRL",
        "nombre_fantasia": "McBurger",
        "sucursal": {
            "direccion": "Amézaga 2100",
            "ciudad": "Montevideo",
            "departamento": "Montevideo",
            "pais": "UY"
        }
    },
    "items": [
        {
            "cantidad":1,
            "concepto": "Pelotas de colores",
            "precio": 4242,
            "indicador_facturacion": 3
        },
        {
            "cantidad":1,
            "concepto": "Alquiler de salón",
            "precio": 450,
            "indicador_facturacion": 3
        }
    ],
    "descuentosRecargos": [
        {
            "es_recargo": false,
            "desc_rec_tipo": "$",
            "glosa": "Descuento pago en fecha",
            "valor": 270,
            "indicador_facturacion": 3
        },
        {
            "es_recargo": false,
            "desc_rec_tipo": "$",
            "glosa": "Promociones",
            "valor": 2990,
            "indicador_facturacion": 3
        }
    ]
}
```

```json
{
    "tipo_comprobante": 101,
    "forma_pago": 1, 
    "sucursal": 1, 
    "moneda": "UYU",
    "montos_brutos": 1,
    "cliente": {
        "tipo_documento": 3, 
        "documento": "47395795",
        "nombre_fantasia": "Santiago Israel",
        "sucursal": {
            "pais": "UY"
        }
    },
    "items": [
        {
            "cantidad": 1,
            "concepto": "PROBANDING",
            "precio": 20000000,
            "indicador_facturacion": 3
        }
    ]
}
```
> Se obtiene la respuesta:

```json
{
    "id": 2873,
    "serie": "B",
    "numero": 133,
    "hash": "KtYEgyy8izLNqoBezxUA53pY92g="
}
```



### HTTP Request

`POST https://test.biller.uy/comprobantes/crear`

### Entrada

Clave | Valor | Detalle
--------- | ---------- | ---------------------
tipo_comprobante | entero | 101 - eTicket<br>102 - Nota de crédito de eTicket<br>103 - Nota de débito de eTicket<br><br>111 - eFactura<br>112 - Nota de crédito de eFactura<br>113 - Nota de débito de eFactura
forma_pago | entero | 1 - contado<br>2 - crédito
fecha_vencimiento | string | Formato dd/mm/aaaa
sucursal|entero|ID de sucursal de la empresa en Biller.<br>Obtener de test o producción
moneda|string|UYU<br>USD<br>ARS<br>BRL<br>EUR<br>...
tasa_cambio|float| Obligatorio si moneda es distinta de UYU 
montos_brutos|booleano| Indica si los precios de los ítems incluyen IVA o no.<br>En el caso de que no incluyan IVA, se calculará y se sumará al total.
comprobante_asociado|entero|(Para notas ajuste)<br>ID del comprobante a asociar.<br>Se puede utilizar este campo o tipo_referencia, serie_referencia y numero_referencia.
tipo_referencia|entero|(Para notas ajuste)<br>Tipo de CFE del comprobante a corregir
serie_referencia|string| (Para notas ajuste)<br>Serie del CFE a corregir
numero_referencia|entero| (Para notas ajuste)<br>Número del CFE a corregir
referencia_global|booleano| (Para notas ajuste)<br>Indica si se utiliza una referencia global o no
razon_referencia|string| (Para notas ajuste)<br>Detalle de razón de referencia. A utilizar por ejemplo en caso de tener que anular comprobantes tradicionales.
numero_orden|string| Número de orden del CFE que se emite
||
**cliente**|objeto|
tipo_documento|entero|2 - RUT<br>3 - CI
documento|string|Valor del RUT o la CI sin puntos ni guiones
razon_social|string|Nombre fiscal de  la empresa.<br>Ej. Arcos Dorados SA
nombre_fantasia|string|Nombre por el que se conoce a la empresa.<br>Ej. McDonald’s
||
**sucursal**|objeto|
direccion|string|
ciudad|string|
departamento|string|
pais|string|UY<br>AR<br>BR<br>US<br>PY<br>CL<br>...
||
**items**|array de objetos|
cantidad|float|
concepto|string|
precio|float (2 decimales)|
indicador_facturacion|entero|1 - Exento de IVA<br>2 - Tasa mínima<br>3 - Tasa básica<br>4 - Otra tasa<br>5 - Entrega gratuita<br>6 - Producto o servicio no facturable<br>7 - Producto o servicio no facturable negativo<br>8 - Ítem a rebajar en e-remitos y en e-remitos de exportación<br>9 - Ítem a anular en resguardos<br>10 - Exportación y asimiladas<br>11 - Impuesto percibido<br>12 - IVA en suspenso
||
**descuentosRecargos**|array de objetos|
es_recargo|booleano|
desc_rec_tipo|string|$ o %
glosa|string|Razón del descuento/recargo
valor|float (2 decimales)|Valor en $ o %
indicador_facturacion|entero|Ver items indicador_facturacion por valores posibles.<br>Valor debe coincidir con el del ítem o ítems que se desea afectar.
||
adenda|string|Cadena a enviar a DGI en la adenda y a mostrar en el PDF

## comprobantes/pdf

> Se obtiene el contenido del archivo PDF codificado en base64:

```json
{
"JVBERi0xLjQKJeLjz9MKMyAwIG9iago8PC9UeXBlIC9QYWdlCi9QYXJlbnQgMSAwIFIKL01lZGlhQm94IFswIDAgNTk1LjI3NiA4NDEuODkwXQovVHJpbUJveCBbMC4wMDAgMC4wMDAgNTk1LjI3NiA4NDEuODkwXQovUmVzb3VyY2VzIDIgMCBSCi9Hcm(…)"
}
```
Devuelve el PDF de la representación impresa del comprobante indicado en el ID.

Este comprobante ya debe existir en el sistema.
### HTTP Request

`GET https://test.biller.uy/comprobantes/pdf`

### Entrada

Clave | Valor | Detalle
--------- | ---------- | ---------------------
id | entero |ID del comprobante a representar en PDF
