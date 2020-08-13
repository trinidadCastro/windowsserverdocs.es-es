---
title: Compatibilidad con ISV de entrega de actualizaciones Express
description: 'Tema de Windows Server Update Services (WSUS): cómo los fabricantes de software independientes (ISV) pueden configurar la entrega de actualizaciones Express con WSUS'
ms.topic: get-started article
author: sakitong
ms.author: coreyp
manager: lizapo
ms.date: 10/16/2017
ms.openlocfilehash: 3c56c9ac0f2899f03ce1b32b699b577c2dc3de0c
ms.sourcegitcommit: 68444968565667f86ee0586ed4c43da4ab24aaed
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87991083"
---
# <a name="express-update-delivery-isv-support"></a>Compatibilidad con ISV de entrega de actualizaciones Express

>Se aplica a: Windows 10, Windows Server 2016

Las descargas de actualizaciones de Windows 10 pueden ser grandes, porque cada paquete contiene todas las revisiones publicadas anteriormente para garantizar la coherencia y simplicidad.

Desde la versión 7, Windows ha podido reducir el tamaño de las descargas de Windows Update con una característica llamada [Express](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc708456(v=ws.10)#Anchor_2) y, aunque los dispositivos de consumo la admiten de forma predeterminada, los dispositivos Windows 10 Enterprise requieren Windows Server Update Services (WSUS) para poder usar Express.

## <a name="how-microsoft-supports-express"></a>Cómo Microsoft admite Express

- **Express en WSUS independiente**

    La entrega de actualizaciones Express ya está [disponible en todas las versiones admitidas de WSUS](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc708456(v=ws.10)).

- **Express en dispositivos conectados directamente a Windows Update**

    Los dispositivos de consumo admiten la descarga Express: usan el cliente de Windows Update (WU) para examinar, descargar e instalar actualizaciones. Durante la fase de descarga, el cliente de WU solicita los paquetes Express y descarga los intervalos de bytes correspondientes.

-  Los **dispositivos empresariales administrados mediante [Windows Update para empresas](/windows/deployment/update/waas-manage-updates-wufb)** también aprovechan las ventajas de la compatibilidad con la entrega de actualizaciones Express sin necesidad de aplicar ningún cambio de configuración.

## <a name="how-isvs-can-take-advantage-of-express"></a>Cómo los ISV pueden aprovechar las ventajas de Express

Los ISV pueden usar WSUS y el cliente de WU para admitir la entrega de actualizaciones de Express. Microsoft recomienda los tres pasos siguientes, que se describen con más detalle en las secciones siguientes:

1.  [**Configurar WSUS**](#BKMK_1)

    El servidor de WSUS es necesario para examinar y actualizar las sincronizaciones (se puede encontrar información adicional [aquí](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn800972(v=ws.11)))

2.  [**Especificar y rellenar una memoria caché de archivos de ISV**](#BKMK_2)

    Se recomienda usar una memoria caché de archivos de ISV para hospedar el contenido de la actualización, que incluye los archivos. cab de actualización y los paquetes Express (archivos .psf).

3.  [**Configurar un agente cliente de ISV para dirigir las operaciones de cliente de WU**](#BKMK_3)

>[!NOTE]
>Requiere la instalación de la actualización Acumulativa para la versión 1607 de Windows 10 de enero de 2017 o posterior ([KB3213986 (OS Build 14393.693)](https://support.microsoft.com/help/4009938/january-10-2017-kb3213986-os-build-14393-693).

   - El agente cliente de ISV determina qué actualizaciones se deben aprobar y cuándo se descargan e instalan las actualizaciones
   - El cliente de WU determina los intervalos de bytes que se van a descargar e inicia la solicitud de descarga

### <a name="step-1-configure-wsus"></a><a name=BKMK_1></a>Paso 1: Configurar WSUS

WSUS actúa como interfaz para Windows Update y administra todos los metadatos que describen los paquetes Express que deben descargarse. Si necesitas implementar, consulta [**Información general de Windows Server Update Services 3.0 SP2**](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd939931(v=ws.10)). Una vez implementado WSUS, la consideración principal es si se debe almacenar o no el contenido de la actualización localmente en el servidor de WSUS. Al configurar WSUS, se recomienda no almacenar las actualizaciones de forma local. Se supone que ya tienes software que dirige la implementación de estos paquetes en tu entorno. Para obtener más información acerca de cómo configurar el almacenamiento local de WSUS, consulta [**Determinar dónde almacenar las actualizaciones**](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc720494(v=ws.10)).

### <a name="step-2-specify-and-populate-the-isv-file-cache"></a><a name=BKMK_2></a>Paso 2: Especificar y rellenar la memoria caché de archivos de ISV

#### <a name="specify-the-isv-file-cache"></a>Especificar la memoria caché de archivos de ISV

Las nuevas opciones de directiva de grupo y administración de dispositivos móviles (MDM) del lado cliente que se detallan en la [**Referencia del proveedor de servicios de configuración**](/windows/client-management/mdm/configuration-service-provider-reference) definen la ubicación de la memoria caché de archivos de ISV.

| **Nombre**                                              | **Descripción**                                                                                                                                                      |
|-------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Configura una ubicación de descarga alternativa para las actualizaciones. | Especifica un servidor de intranet alternativo para las actualizaciones de host de Microsoft Update. A continuación, puedes usar este servicio de actualización para actualizar automáticamente los equipos de la red |

Hay dos opciones al configurar la ubicación de descarga alternativa para la memoria caché de archivos de ISV:

1. **Especificar un nombre de host de servidor HTTP de ISV**, que es la memoria caché de archivos de ISV

    Este enfoque configura el cliente de WU para que realice solicitudes de descarga en el servidor HTTP especificado en la directiva.

2. **Especifica un localhost**

    Este enfoque configura el cliente de WU para que realice solicitudes de descarga en el localhost. Esto permite al agente cliente de ISV controlar estas solicitudes y enrutarlas según corresponda para completar la solicitud de descarga.

> [!IMPORTANT]
> La memoria caché de archivos de ISV requiere lo siguiente:
> - El servidor debe ser compatible con HTTP 1.1 según el protocolo RFC <http://www.w3.org/Protocols/rfc2616/rfc2616.html>. En concreto, el servidor web debe admitir las solicitudes                                                                                                                                                                                                                                       [**HEAD**](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html) y [**GET**](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.htm).<br>                                                                                                                                                                                                                                                                                                  - Solicitudes de intervalo parcial<br>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   - Persistentes<br>                                                                                                                                                                                                                                                                                                                                                                                                                            - No usar Transfer-Encoding:chunked

#### <a name="populate-the-isv-file-cache"></a>Rellenar la memoria caché de archivos de ISV

La memoria caché de archivos de ISV se debe rellenar con los archivos asociados con las actualizaciones que se instalarán en los clientes administrados.

**Para rellenar la memoria caché de archivos de ISV:**

1. Usa la [API de WSUS](/previous-versions/windows/desktop/aa354524(v=vs.85)) para tener acceso a la ruta de acceso y al nombre de archivo de la actualización para el servicio MU.

    Los metadatos de cada actualización en el servidor de WSUS contienen la ruta de acceso y el nombre de archivo de la actualización en Microsoft Update de la siguiente forma (nombre de host de Microsoft Update en negrita, seguido de la ruta de acceso y el nombre de archivo): **<http://download.windowsupdate.com>** /c/msdownload/update/software/updt/2016/09/windows10.0-kb3195781-x64_0c06079bccc35cba35a48bd2b1ec46f818bd2e74.msu

2. Descarga archivos de Microsoft Update y almacénalos en la memoria caché de archivos de ISV mediante uno de estos dos métodos:

   - Almacenar archivos con la **misma ruta de acceso de carpeta que el servicio de MU**

   - Almacenar archivos mediante una **ruta de acceso de carpeta definida por el ISV**

     Haz que el servidor HTTP (o localhost) redirija las solicitudes **HTTP GET**, que hacen referencia a la ruta de acceso de la carpeta de MU y al nombre de archivo en la ubicación del archivo de ISV.

### <a name="step-3-set-up-an-isv-client-agent-to-direct-wu-client-operations"></a><a name=BKMK_3></a>Paso 3: Configurar un agente cliente de ISV para dirigir las operaciones de cliente de WU

El agente cliente de ISV organiza la descarga y la instalación de las actualizaciones aprobadas mediante el siguiente flujo de trabajo recomendado:

1.  El agente cliente de ISV llama al cliente de WU para realizar el análisis en el servidor de WSUS

2.  El examen devuelve el conjunto de actualizaciones aplicables al cliente de WU

3.  El cliente de ISV determina qué actualizaciones se deben aprobar, descargar e instalar

4.  El agente cliente de ISV llama al cliente de WU para descargar las actualizaciones aprobadas

5.  Una vez descargadas las actualizaciones, el agente cliente de ISV llama al cliente de WU para instalar las actualizaciones aprobadas.

Consulta [Búsqueda, descarga e instalación de actualizaciones](/windows/win32/wua_sdk/searching--downloading--and-installing-updates) para obtener información adicional sobre cómo usar el cliente de WU para examinar, descargar e instalar actualizaciones.

### <a name="download-workflow-options"></a>Descargar opciones de flujo de trabajo

A continuación se muestran dos ilustraciones de opciones de flujo de trabajo de descarga desde una memoria caché de archivos de ISV:

![Flujo de trabajo 1](../../media/express-update-delivery-isv-support/image1.png)

![Flujo de trabajo 2](../../media/express-update-delivery-isv-support/image2.png)
### <a name="how-express-download-works"></a>Cómo funciona la descarga de Express

- Para las actualizaciones del sistema operativo que admiten Express, existen dos versiones de la carga de archivos almacenada en el servicio:

  - **Versión de archivo completo**: básicamente, reemplaza las versiones locales de los archivos binarios de la actualización.

  - **Versión Express**: contiene las diferencias necesarias para la revisión de los archivos binarios existentes en el dispositivo.

    Los metadatos de la actualización, que se han descargado en el cliente como parte de la fase de examen, hacen referencia tanto a la versión de archivo completo como a la versión Express.

    **La descarga Express funciona del siguiente modo**:

    El cliente de WU intentará primero la descarga Express y, en determinadas situaciones, recurrirá a la versión de archivo completo si es necesario (por ejemplo, si pasa por un proxy que no admite las solicitudes de intervalo de bytes).

  1. Cuando el cliente de WU inicia una descarga Express, **el cliente de WU descarga primero un código auxiliar**, que forma parte del paquete Express.

  2. **El cliente de WU pasa este código auxiliar a Windows Installer**, que usa el código auxiliar para hacer un inventario local, comparar las diferencias del archivo en el dispositivo con lo que se necesita para acceder a la versión más reciente del archivo que se ofrece.

  3. **Luego, Windows Installer solicita al cliente de WU que descargue los intervalos** que se ha determinado que son necesarios.

  4. **El cliente de WU descarga estos intervalos y los pasa a Windows Installer**, que aplica los intervalos y, luego, determina si se necesitan intervalos adicionales. Esto se repite hasta que Windows Installer indica al cliente de WU que se han descargado todos los intervalos necesarios.

  En este punto, se completa la descarga y la actualización está lista para instalarse.

### <a name="how-delivery-optimization-reduces-bandwidth-consumption"></a>Cómo la optimización de distribución reduce el consumo de ancho de banda

La Optimización de distribución (DO) es una solución de caché distribuida autoorganizativa para empresas que quieren reducir el consumo de ancho de banda para las actualizaciones del sistema operativo y las aplicaciones. DO permite a los clientes descargar esos elementos de fuentes alternativas (como otros elementos del mismo nivel en la red) junto con la ubicación de descarga especificada (la memoria caché de archivos de ISV, en este escenario).

De manera predeterminada en Windows 10 Enterprise y Education, DO solo permite el uso compartido punto a punto en la red de la organización, pero puedes configurarla de forma diferente desde la configuración de la directiva de grupo y la administración de dispositivos móviles (MDM).

Consulta [Configuración de la Optimización de distribución para actualizaciones de Windows 10](/windows/deployment/update/waas-delivery-optimization) para obtener más información acerca de DO.