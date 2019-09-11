---
title: Soporte técnico de ISV de entrega de actualizaciones Express
description: 'Tema de Windows Server Update Services (WSUS): cómo los fabricantes de software independientes (ISV) pueden configurar la entrega rápida de actualizaciones con WSUS'
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
ms.openlocfilehash: 0f5893d47219e9263ed7f35bee472848a47c6164
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2019
ms.locfileid: "70868738"
---
# <a name="express-update-delivery-isv-support"></a>Soporte técnico de ISV de entrega de actualizaciones Express

>Se aplica a: Windows 10, Windows Server 2016

Las descargas de actualizaciones de Windows 10 pueden ser grandes porque cada paquete contiene todas las correcciones publicadas anteriormente para garantizar la coherencia y simplicidad.  

Desde la versión 7, Windows ha podido reducir el tamaño de las descargas de Windows Update con una característica denominada [Express](https://technet.microsoft.com/library/cc708456(v=ws.10).aspx#Anchor_2)y, aunque los dispositivos de consumidor lo admiten de forma predeterminada, los dispositivos de Windows 10 enterprise requieren Windows Server Update Services (WSUS) para realizar ventaja de Express.

## <a name="how-microsoft-supports-express"></a>Cómo Microsoft admite Express

- **Express en WSUS independiente**

    La entrega de actualizaciones rápidas ya está [disponible en todas las versiones compatibles de WSUS](https://technet.microsoft.com/library/cc708456(v=ws.10).aspx).

- **Express en dispositivos conectados directamente a Windows Update** 

    Los dispositivos de consumidor admiten la descarga rápida: usan el cliente de Windows Update (WU) para examinar, descargar e instalar actualizaciones. Durante la fase de descarga, el cliente de WU solicita los paquetes Express y descarga los intervalos de bytes correspondientes.

-  **Los dispositivos de empresa administrados a través de [Windows Update para empresas](https://technet.microsoft.com/itpro/windows/manage/waas-manage-updates-wufb)** también aprovechan las ventajas de soporte técnico de entrega de actualizaciones Express sin realizar cambios en la configuración.

## <a name="how-isvs-can-take-advantage-of-express"></a>Cómo los ISV pueden aprovechar las ventajas de Express

Los ISV pueden usar WSUS y el cliente de WU para admitir la entrega rápida de actualizaciones. Microsoft recomienda los tres pasos siguientes, cada uno de los cuales se describe con más detalle en las secciones siguientes:

1.  [**Configurar WSUS**](#BKMK_1)

    El servidor WSUS es necesario para la exploración & sincronizaciones de actualizaciones ( [aquí](https://technet.microsoft.com/library/dn800972(v=ws.11).aspx)encontrará información adicional)

2.  [**Especificar y rellenar una memoria caché de archivos de ISV**](#BKMK_2)

    Se recomienda usar una memoria caché de archivos de ISV para hospedar el contenido de la actualización, que incluye los archivos. cab de actualización (archivos. cab) y los paquetes Express (archivos. PSF).

3.  [**Configurar un agente cliente de ISV para dirigir las operaciones de cliente de WU**](#BKMK_3)

>[!NOTE]
>Requiere la actualización acumulativa de la versión 1607 de Windows 10 en (o posterior) 2017 de enero ([KB3213986 (compilación de so 14393,693)](https://support.microsoft.com/en-us/help/4009938/january-10-2017-kb3213986-os-build-14393-693) .
    
   - El agente cliente de ISV determina qué actualizaciones se deben aprobar y cuándo se descargan e instalan las actualizaciones.
   - El cliente WU determina los intervalos de bytes que se van a descargar e inicia la solicitud de descarga

### <a name="BKMK_1"></a>Paso 1: Configurar WSUS

WSUS actúa como la interfaz para Windows Update y administra todos los metadatos que describen los paquetes Express que deben descargarse. Si necesita implementar, consulte [**información general de Windows Server Update Services 3,0 SP2**](https://technet.microsoft.com/library/dd939931(v=ws.10).aspx). Una vez implementado WSUS, la consideración principal es si se debe almacenar o no el contenido de actualización localmente en el servidor WSUS. Al configurar WSUS, se recomienda no almacenar las actualizaciones de forma local. Se supone que ya tiene software que dirige la implementación de estos paquetes en su entorno. Para obtener más información acerca de cómo configurar el almacenamiento local de WSUS, consulte [**determinar dónde almacenar las actualizaciones**](https://technet.microsoft.com/library/cc720494(v=ws.10).aspx).

### <a name="BKMK_2"></a>Paso 2: Especificar y rellenar la memoria caché de archivos de ISV 

#### <a name="specify-the-isv-file-cache"></a>Especificar la memoria caché de archivos de ISV

Las nuevas opciones de directiva de grupo y administración de dispositivos móviles (MDM) del lado cliente [**que se**](https://msdn.microsoft.com/windows/hardware/commercialize/customize/mdm/configuration-service-provider-reference) detallan en la referencia del proveedor de servicios de configuración definen la ubicación de la memoria caché de archivos de ISV.

| **Name**                                              | **Descripción**                                                                                                                                                      |
|-------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Configurar una ubicación de descarga alternativa para las actualizaciones. | Especifica un servidor de intranet alternativo del que se van a hospedar las actualizaciones de Microsoft Update. Después, puede usar este servicio de actualización para actualizar automáticamente los equipos de la red. |

Hay dos opciones al configurar la ubicación de descarga alternativa para la memoria caché de archivos de ISV:

1. **Especifique un nombre de host de servidor HTTP de ISV**, que es la memoria caché de archivos de ISV
    
    Este enfoque configura el cliente de WU para que realice solicitudes de descarga en el servidor HTTP especificado en la Directiva.

2. **Especificar localhost**
 
    Este enfoque configura el cliente de WU para que realice solicitudes de descarga en localhost. Esto permite al agente cliente de ISV controlar estas solicitudes y enrutar según corresponda para completar la solicitud de descarga.

> [!IMPORTANT]
> La memoria caché de archivos de ISV requiere lo siguiente:                                                          
> - El servidor debe ser compatible con HTTP 1,1 según la RFC:<http://www.w3.org/Protocols/rfc2616/rfc2616.html>                                                                                                                                                                
> En concreto, el servidor Web debe admitir                                                                                                                                                                                                                                       Solicitudes [**Head**](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html) y [**Get**](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.htm)<br>                                                                                                                                                                                                                                                                                                  -Solicitudes de intervalo parcial<br>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   -Keep-Alive<br>                                                                                                                                                                                                                                                                                                                                                                                                                            -No usar "codificación de transferencia: fragmentada"                                                                                                 

#### <a name="populate-the-isv-file-cache"></a>Rellenar la memoria caché de archivos de ISV

La memoria caché de archivos de ISV se debe rellenar con los archivos asociados con las actualizaciones que se instalarán en los clientes administrados. 

**Para rellenar la memoria caché de archivos de ISV:**

1. Use las [API de WSUS](https://msdn.microsoft.com/library/windows/desktop/microsoft.updateservices.administration.updatefile(v=vs.85).aspx) para acceder a la ruta de acceso y el nombre de archivo de la actualización para el servicio mu.

    Los metadatos de cada actualización en el servidor WSUS contienen la ruta de acceso y el nombre de archivo de la actualización en Microsoft Update de la siguiente forma (Microsoft Update nombre de host en **<http://download.windowsupdate.com>** negrita, seguido de la ruta de acceso y el nombre de archivo):/c/msdownload/update/software/UPDT/2016/09/ Windows 10.0-kb3195781-x64_0c06079bccc35cba35a48bd2b1ec46f818bd2e74. msu

2. Descargue archivos de Microsoft Update y almacénelos en la memoria caché de archivos de ISV mediante uno de estos dos métodos: 

   - Almacenar archivos con la **misma ruta de acceso de la carpeta que en el servicio MU**

   - Almacenar archivos mediante una **ruta de carpeta definida por ISV**

     Hacer que el servidor HTTP (o localhost) redirija las solicitudes **http Get** , que hacen referencia a la ruta de acceso de la carpeta MU y al nombre de archivo, a la ubicación del archivo ISV.

### <a name="BKMK_3"></a>Paso 3: Configurar un agente cliente de ISV para dirigir las operaciones de cliente de WU

El agente cliente de ISV organiza la descarga e instalación de las actualizaciones aprobadas mediante el siguiente flujo de trabajo recomendado:

1.  El agente cliente de ISV llama al cliente de WU para realizar el análisis en el servidor WSUS

2.  El análisis devuelve el conjunto de actualizaciones aplicables al cliente de WU

3.  El cliente de ISV determina qué actualizaciones se deben aprobar, descargar e instalar.

4.  El agente de cliente de ISV llama al cliente de WU para descargar las actualizaciones aprobadas

5.  Una vez descargadas las actualizaciones, el agente cliente de ISV llama al cliente de WU para instalar las actualizaciones aprobadas.

Consulte [Buscar, descargar e instalar actualizaciones](https://msdn.microsoft.com/library/windows/desktop/aa387102(v=vs.85).aspx) para obtener información adicional sobre cómo usar el cliente de Wu para examinar, descargar e instalar actualizaciones.

### <a name="download-workflow-options"></a>Descargar opciones de flujo de trabajo

A continuación se muestran dos ilustraciones de opciones de flujo de trabajo de descarga desde una memoria caché de archivos de ISV:

![Flujo de trabajo 1](../../media/express-update-delivery-isv-support/image1.png)

![Flujo de trabajo 2](../../media/express-update-delivery-isv-support/image2.png)
### <a name="how-express-download-works"></a>Cómo funciona la descarga Express

- Para las actualizaciones del sistema operativo que admiten Express, existen dos versiones de la carga de archivos almacenada en el servicio:

  - **Versión de archivo completo** : reemplazando esencialmente las versiones locales de los binarios de actualización

  - **Versión Express** : contiene los deltas necesarios para aplicar revisiones a los archivos binarios existentes en el dispositivo. 

    Se hace referencia a la versión de archivo completo y a la versión Express en los metadatos de la actualización, que se han descargado en el cliente como parte de la fase de examen. 

    **La descarga rápida funciona de la siguiente manera:**

    El cliente de WU intentará descargar Express primero y, en determinadas situaciones, revertir a un archivo completo si es necesario (por ejemplo, si se pasa a través de un proxy que no admite solicitudes de intervalo de bytes).

  1. Cuando el cliente de WU inicia una descarga rápida, **el cliente de Wu descarga primero un código auxiliar**, que forma parte del paquete Express.

  2. **El cliente de Wu pasa este código auxiliar a Windows Installer**, que usa el código auxiliar para realizar un inventario local, comparando las diferencias del archivo en el dispositivo con lo que se necesita para llegar a la versión más reciente del archivo que se está ofreciendo.

  3. **Después, Windows Installer solicita al cliente de Wu que descargue los intervalos** que se han determinado como obligatorios.

  4. **El cliente de Wu descarga estos intervalos y los pasa a Windows Installer**, que aplica los intervalos y, a continuación, determina si se necesitan intervalos adicionales. Esto se repite hasta que Windows Installer indica al cliente de WU que se han descargado todos los intervalos necesarios.

  En este punto, se completa la descarga y la actualización está lista para instalarse.

### <a name="how-delivery-optimization-reduces-bandwidth-consumption"></a>Cómo la optimización de entrega reduce el consumo de ancho de banda

La optimización de entrega (DO) es una solución de caché distribuida autoorganizativa para empresas que desean reducir el consumo de ancho de banda para las actualizaciones del sistema operativo, las actualizaciones del sistema operativo y las aplicaciones. Permite a los clientes descargar esos elementos de orígenes alternativos (como otros elementos del mismo nivel en la red) junto con la ubicación de descarga especificada (la memoria caché de archivos de ISV en este escenario).

De forma predeterminada en Windows 10 Enterprise y Education, permite el uso compartido punto a punto solo en la propia red de la organización, pero se puede configurar de forma diferente mediante la configuración de directiva de grupo y de la administración de dispositivos móviles (MDM).

Consulte [configuración de la optimización de distribución para actualizaciones de Windows 10](https://technet.microsoft.com/itpro/windows/manage/waas-delivery-optimization) para obtener más información acerca de cómo hacerlo.
