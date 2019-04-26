---
title: Soporte técnico de ISV de entrega de actualizaciones Express
description: Tema de Windows Server Update Service (WSUS) - cómo independientes de Software proveedores (ISV) puede configurar la entrega de actualización de Express con WSUS
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-wsus
ms.tgt_pltfrm: na
ms.topic: get-started article
author: sakitong
ms.author: coreyp
manager: lizapo
ms.date: 10/16/2017
ms.openlocfilehash: b891f61ff2c930591c33805d0e3bc595ebf196f7
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59850496"
---
#<a name="express-update-delivery-isv-support"></a>Soporte técnico de ISV de entrega de actualizaciones Express

>Se aplica a: Windows 10, Windows Server 2016

Descargas de actualización de Windows 10 pueden ser grandes, porque cada paquete contiene todas las revisiones publicadas anteriormente para garantizar la coherencia y la simplicidad.  

Desde la versión 7, Windows ha sido capaz de reducir el tamaño de las descargas de Windows Update con una característica denominada [Express](https://technet.microsoft.com/library/cc708456(v=ws.10).aspx#Anchor_2), y aunque dispositivos para consumidores admiten de forma predeterminada, los dispositivos de Windows 10 enterprise requieren Windows Server Update Services (WSUS) para aprovechar las ventajas de Express.

## <a name="how-microsoft-supports-express"></a>Cómo Microsoft admite Express

- **Express en WSUS independiente**

    Entrega de actualizaciones Express ya está [disponible en todas las versiones compatibles de WSUS](https://technet.microsoft.com/library/cc708456(v=ws.10).aspx).

- **Express en los dispositivos conectados directamente a Windows Update** 

    Los dispositivos de consumidor admiten descarga Express: uso del cliente de Windows Update (WU) para buscar, descargar e instalar actualizaciones. Durante la fase de descarga, el cliente WU solicita los paquetes de Express y descarga los intervalos de bytes adecuado.

-  **Los dispositivos de empresa administrados a través de [Windows Update para empresas](https://technet.microsoft.com/itpro/windows/manage/waas-manage-updates-wufb)** también aprovechan las ventajas de soporte técnico de entrega de actualizaciones Express sin realizar cambios en la configuración.

##<a name="how-isvs-can-take-advantage-of-express"></a>Cómo los ISV pueden aprovechar las ventajas de Express

Los ISV pueden utilizar WSUS y el cliente de Windows Update para admitir la entrega de actualización de Express. Microsoft recomienda a los tres pasos siguientes, cada uno describe con más detalle en las secciones siguientes:

1.  [**Configurar WSUS**](#BKMK_1)

    Servidor WSUS es necesaria para analizar & sincronizaciones de actualizaciones (puede encontrar información adicional [aquí](https://technet.microsoft.com/library/dn800972(v=ws.11).aspx))

2.  [**Especificar y rellenar una caché de archivos de ISV**](#BKMK_2)

    Se recomienda una memoria caché de archivos de ISV para hospedar el contenido de actualización, que incluye los archivos CAB de actualización (archivos .cab) y la Express paquetes (archivos .psf).

3.  [**Configurar un agente de cliente de ISV para dirigir las operaciones de cliente de Windows Update**](#BKMK_3)

>[!NOTE]
>Requiere la versión de acumulativa Update para Windows 10 versión 1607 en (o después) de enero de 2017 ([KB3213986 (14393.693 de compilación del sistema operativo)](https://support.microsoft.com/en-us/help/4009938/january-10-2017-kb3213986-os-build-14393-693) para instalarse.
    
   - El agente de cliente de ISV determina qué actualizaciones para aprobar y cuando descargar e instalar actualizaciones
   - El cliente WU determina los intervalos de bytes para descargar e inicia la solicitud de descarga

### <a name="BKMK_1"></a>Paso 1: Configurar WSUS

WSUS actúa como la interfaz de Windows Update y administra todos los metadatos que describe los paquetes de Express que deben descargarse. Si tiene que implementar, consulte [ **información general de Windows Server Update Services 3.0 SP2**](https://technet.microsoft.com/library/dd939931(v=ws.10).aspx). Una vez que se ha implementado WSUS, la consideración principal es almacenar el contenido de actualización localmente en el servidor WSUS o no. Al configurar WSUS, se recomienda no almacenar actualizaciones localmente. Esto supone que ya tiene software dirigir la implementación de estos paquetes en su entorno. Para obtener más información acerca de cómo configurar el almacenamiento local de WSUS, consulte [ **determinar dónde Store actualizaciones**](https://technet.microsoft.com/library/cc720494(v=ws.10).aspx).

### <a name="BKMK_2"></a>Paso 2: Especificar y rellenar la caché de archivos de ISV 

####<a name="specify-the-isv-file-cache"></a>Especificar la caché de archivos de ISV

Nueva configuración de directiva de grupo y administración de dispositivos móviles (MDM) de cliente detallada en el [ **referencia del proveedor de servicio de configuración** ](https://msdn.microsoft.com/en-us/windows/hardware/commercialize/customize/mdm/configuration-service-provider-reference) definen la ubicación de la memoria caché de archivos de ISV.

| **Name**                                              | **Descripción**                                                                                                                                                      |
|-------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Configurar una ubicación de descarga alternativa para las actualizaciones. | Especifica un servidor de intranet alternativo para alojar actualizaciones desde Microsoft Update. A continuación, puede usar este servicio de actualización para actualizar automáticamente los equipos de la red |

Hay dos opciones al configurar la ubicación de descarga alternativo para la caché de archivos de ISV:

1. **Especifique un nombre de host del servidor HTTP de ISV**, que es la memoria caché de archivos de ISV
    
    Este enfoque configura el cliente de Windows Update para que se descarguen las solicitudes al servidor HTTP especificado en la directiva

2. **Especifique localhost**
 
    Este enfoque configura el cliente de Windows Update para que se descarguen las solicitudes al host local. Esto permite al agente de cliente de ISV controlar estas solicitudes y la ruta según corresponda satisfacer la solicitud de descarga.

>[!IMPORTANT]
>La memoria caché de archivos de ISV requiere lo siguiente:                                                          
                                                                                                                                   >- El servidor debe ser conforme según la RFC de HTTP 1.1: <http://www.w3.org/Protocols/rfc2616/rfc2616.html>                                                                                                                                                                
                                                                                                                                   >En concreto, debe admitir el servidor web                                                                                                                                                                                                                                       [ **HEAD** ](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html) y [ **obtener** ](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.htm) solicitudes<br>                                                                                                                                                                                                                                                                                                  -Solicitudes de intervalo de parcial<br>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   -Keep-alive<br>                                                                                                                                                                                                                                                                                                                                                                                                                            ¿-No use "Transfer-Encoding: fragmentada?                                                                                                 

#### <a name="populate-the-isv-file-cache"></a>Rellenar la caché de archivos de ISV

La memoria caché de archivos de ISV se debe rellenar con los archivos asociados con las actualizaciones se instale en los clientes administrados. 

**Para rellenar la caché de archivos de ISV:**

1. Use [las API de WSUS](https://msdn.microsoft.com/en-us/library/windows/desktop/microsoft.updateservices.administration.updatefile(v=vs.85).aspx) para tener acceso a la actualización de la ruta de acceso y nombre de archivo para el servicio Microsoft Update.

    Los metadatos para cada actualización en el servidor WSUS contienen la actualización de la ruta de acceso y nombre de archivo en Microsoft Update como se indica a continuación (nombre de host de Microsoft Update en negrita, seguido de la ruta de acceso y nombre de archivo): **http://download.windowsupdate.com** /c/msdownload/actualización / software/updt/2016/09/windows10.0-kb3195781-x64_0c06079bccc35cba35a48bd2b1ec46f818bd2e74.msu

2. Descargar archivos desde Microsoft Update y almacenarlos en la memoria caché de archivos de ISV mediante uno de estos dos métodos: 

 - Archivos de Store mediante el **misma ruta de carpeta como en el servicio MU**

 - Archivos de Store mediante una **ruta de acceso de carpeta definido por el ISV**

    Tiene la redirección HTTP server (o localhost) **HTTP GET** solicitudes, que hacen referencia a la MU ruta de acceso y nombre de la carpeta, en la ubicación del archivo de ISV.

### <a name="BKMK_3"></a>Paso 3: Configurar un agente de cliente de ISV para dirigir las operaciones de cliente de Windows Update

El agente de cliente de ISV Orquesta la descarga e instalación de actualizaciones aprobadas mediante el flujo de trabajo recomendado siguiente:

1.  El agente de cliente de ISV llama al cliente de Windows Update para buscar en el servidor WSUS

2.  El examen devuelve el conjunto de las actualizaciones aplicables al cliente WU

3.  El cliente de ISV determina qué actualizaciones se apruebe, descargue e instale

4.  El cliente WU ISV llamadas de agente de cliente para descargar las actualizaciones aprobadas

5.  Una vez que se han descargado las actualizaciones, el agente de cliente de ISV llama al cliente de Windows Update para instalar las actualizaciones aprobadas

Consulte [buscar, descargar e instalar actualizaciones](https://msdn.microsoft.com/en-us/library/windows/desktop/aa387102(v=vs.85).aspx) para obtener más información acerca de cómo utilizar el cliente de Windows Update para examinar, descargar e instalar actualizaciones.

### <a name="download-workflow-options"></a>Opciones de flujo de trabajo de descarga

Estas son las dos ilustraciones de las opciones de flujo de trabajo de descarga de una memoria caché de archivos de ISV:

![Flujo de trabajo 1](../../media/express-update-delivery-isv-support/image1.png)

![Flujo de trabajo 2](../../media/express-update-delivery-isv-support/image2.png)
### <a name="how-express-download-works"></a>Cómo funciona la descarga Express

- Para las actualizaciones del sistema operativo que admiten Express, existen dos versiones de la carga de archivos almacenada en el servicio:

 - **Versión de archivo completo** --esencialmente reemplazando las versiones locales de los archivos binarios de actualización

 - **Versión Express** --que contiene los deltas necesaria para aplicar revisiones a los archivos binarios existentes en el dispositivo. 

   La versión de archivo completo y la versión Express se hace referencia en los metadatos de la actualización, que se ha descargado en el cliente como parte de la fase de análisis. 

   **Descarga rápida funciona del siguiente modo:**

   El cliente WU intentará descargar Express en primer lugar y en determinadas situaciones de otoño a todos los archivos de la si es necesario (por ejemplo, si va a través de un proxy que no es compatible con solicitudes de intervalo de bytes).

  1. Cuando el cliente WU inicia una descarga rápida, **el cliente WU primero descarga un código auxiliar**, que forma parte del paquete de Express.

  2. **El cliente WU pasa este código auxiliar para el programa de instalación de Windows**, que usa el código auxiliar para realizar un inventario local, comparar las diferencias del archivo en el dispositivo con lo que se necesita para llegar a la versión más reciente del archivo que se ofrecen.

  3. El **instalador de Windows, a continuación, solicita el cliente de Windows Update para descargar los intervalos** que se ha determinado que se requiere.

  4. **El cliente de Windows Update descarga estos intervalos y los pasa al instalador de Windows**, que se aplica a los intervalos y, a continuación, determina si se necesitan intervalos adicionales. Esto se repite hasta que el programa de instalación de Windows indica que el cliente de Windows Update que se han descargado todos los intervalos necesarios.

  En este punto, se completa la descarga y la actualización está lista para instalarse.

### <a name="how-delivery-optimization-reduces-bandwidth-consumption"></a>La optimización de entrega reduce el consumo de ancho de banda

Optimización de entrega (DO) es una solución de caché distribuida Auto-organiza para las empresas que desean para reducir el consumo de ancho de banda de las actualizaciones del sistema operativo, actualizaciones del sistema operativo y aplicaciones. Permite a los clientes descargar los elementos de orígenes alternativos (como otros elementos del mismo nivel en la red) junto con la ubicación de descarga especificada (la caché de archivos de ISV en este escenario).

De forma predeterminada en Windows 10 Enterprise y Education, permite el uso compartido de punto a punto en la red de la organización solo, pero puede configurarlo diferente mediante la directiva de grupo y configuración de dispositivos móviles (MDM) de administración.

Consulte [actualiza la configuración de optimización de entrega para Windows 10](https://technet.microsoft.com/itpro/windows/manage/waas-delivery-optimization) para obtener más información acerca de.
