# EIGRP Config Generator

Generador web, ligero y sin dependencias para crear configuraciones básicas de **EIGRP para Cisco IOS** a partir de una lista de redes LAN y enlaces WAN.

La herramienta funciona completamente en el navegador: no requiere servidor, instalación, compilación ni conexión a Internet una vez descargado el archivo HTML.

## Características

- Generación de comandos EIGRP por router.
- Soporte para múltiples redes LAN y WAN.
- Asociación automática de enlaces WAN con routers a partir de sus nombres.
- Configuración del número de sistema autónomo (AS) entre `1` y `65535`.
- Wildcard mask en formato decimal o como prefijo CIDR, por ejemplo `/19`.
- Aplicación de una wildcard a un bloque de redes y soporte para varios bloques.
- Copia de comandos individuales o de la configuración completa de cada router.
- Interfaz adaptable a escritorio y dispositivos móviles.
- Tema claro u oscuro según la preferencia del sistema operativo.
- Sin frameworks, dependencias externas ni procesamiento en un backend.

## Requisitos

Solo necesitas un navegador moderno con soporte para JavaScript y la API del portapapeles.

Navegadores recomendados:

- Google Chrome o Chromium
- Mozilla Firefox
- Microsoft Edge
- Safari

## Uso

1. Descarga o clona este repositorio.
2. Abre [`Generador de configuración EIGRP.html`](./Generador%20de%20configuraci%C3%B3n%20EIGRP.html) directamente en el navegador.
3. Introduce las redes y sus nombres en el campo **Texto de entrada**.
4. Define el **AS de EIGRP**.
5. Revisa los comandos generados y utiliza **Copiar** o **Copiar todo**.

No se cargan datos a ningún servidor: toda la conversión se realiza localmente en el navegador.

## Formato de entrada

Cada red debe identificarse como `LAN` o `WAN` y puede escribirse en dos líneas:

```text
LAN LIMA
172.16.0.0
LAN IQUITOS
172.16.64.0
WAN LIM-CEN
172.16.96.0
MASCARA WILDCARD
0.0.31.255
```

También se admite el nombre y la dirección en la misma línea, separados por espacios o `:`:

```text
LAN LIMA: 172.16.0.0
WAN LIM-CEN 172.16.96.0
```

La wildcard puede expresarse como dirección:

```text
MASCARA WILDCARD
0.0.31.255
```

O como prefijo:

```text
MASCARA WILDCARD /19
```

El marcador de wildcard también reconoce `WILDCARD` y variantes de `MASCARA`. Si se omite, la aplicación utiliza `0.0.0.255` como valor predeterminado.

## Ejemplo de configuración generada

Para una red `LAN LIMA` con dirección `172.16.0.0`, wildcard `0.0.31.255` y AS `10`, se generan comandos como:

```text
enable
configure terminal
router eigrp 10
network 172.16.0.0 0.0.31.255
no auto-summary
exit
write memory
```

## Agrupación de routers

La herramienta crea un router para cada bloque `LAN`. Los enlaces `WAN` se asignan a los routers cuyos nombres coinciden total o parcialmente con los identificadores del enlace.

Por ejemplo, `WAN LIM-CEN` puede asociarse con `LIM` y crear un router central `CEN` cuando ese identificador no corresponde a una LAN existente. Por ello, se recomienda utilizar nombres consistentes y abreviaturas de al menos dos caracteres:

```text
LAN LIMA
WAN LIM-CEN
WAN LIM-IQU
```

Las líneas que no empiezan por `LAN` o `WAN`, o que no contienen una dirección reconocible, se ignoran y se muestran como advertencias en la interfaz.

## Estructura del proyecto

```text
.
├── Generador de configuración EIGRP.html
└── README.md
```

El archivo HTML contiene la estructura, los estilos y la lógica de la aplicación, por lo que puede distribuirse como un único archivo.

## Desarrollo

No hay un proceso de build ni dependencias que instalar. Para modificar la herramienta:

1. Edita `Generador de configuración EIGRP.html`.
2. Abre el archivo en un navegador.
3. Prueba cambios en el campo de entrada y valida los comandos resultantes.

## Consideraciones

- La salida está pensada como una base para Cisco IOS; revísala antes de aplicarla en producción.
- La asociación de enlaces WAN depende de los nombres utilizados en la entrada.
- La herramienta genera comandos `write memory`, por lo que conviene validar la configuración antes de copiarla a un equipo.
- El acceso al portapapeles puede depender de los permisos del navegador y del contexto desde el que se abre el archivo.

## Licencia

Este repositorio no incluye actualmente un archivo de licencia. Consulta al propietario antes de redistribuir o incorporar el código en otros proyectos.

## Autor

Desarrollado por [Luxzayk](https://github.com/Luxzayk).
