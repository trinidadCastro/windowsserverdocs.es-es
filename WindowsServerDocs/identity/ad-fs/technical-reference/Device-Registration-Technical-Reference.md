---
description: 'Más información sobre: referencia técnica del registro de dispositivos'
ms.assetid: 69ec592a-5499-4249-8ba0-afa356a8ff75
title: Referencia técnica del registro de dispositivos
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: abe97d43e75bee50374334e33ffb8a29787ae688
ms.sourcegitcommit: 65b6de6b44d41f1180c45db11cdd60cb2a093b46
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/10/2020
ms.locfileid: "97048073"
---
# <a name="device-registration-technical-reference"></a>Referencia técnica del registro de dispositivos
El servicio de registro de dispositivos \( DRS \) es un nuevo servicio de Windows que se incluye con el rol de Servicio de federación de Active Directory en Windows Server 2012 R2.  El DRS debe estar instalado y configurado en todos los servidores de federación en la granja de AD FS.  Para obtener información sobre la implementación de DRS, consulte [Configuración de un servidor de federación con el servicio de registro de dispositivos](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn486831(v=ws.11)).

## <a name="active-directory-objects-created-when-a-device-is-registered"></a>Objetos de Active Directory creados cuando se registra un dispositivo
Los siguientes objetos de Active Directory se crean como parte del servicio de registro de dispositivos.

### <a name="device-registration-configuration"></a>Configuración del registro de dispositivos
La configuración del registro del dispositivos se almacena en el contexto de nomenclatura de configuración del bosque de Active Directory. \(Por ejemplo, **\= configuración del registro de dispositivos CN, servicios cn \= , <contexto de nomenclatura de configuración \- \->** \) . Este objeto se crea cuando se inicia el bosque de Active Directory para el registro de dispositivos.

La configuración del registro del dispositivo incluye los siguientes elementos:

-   **Claves de emisor**

    Claves públicas y privadas usadas para emitir el certificado X.509 asociado con un dispositivo registrado.  Las claves privadas están protegidas mediante DKM.

-   **Configuración del servicio de registro de dispositivos**

    Directivas relacionadas con el servicio de registro de dispositivos.

### <a name="registered-devices-container"></a>Contenedor de dispositivos registrados
El contenedor de objetos de dispositivo se crea en uno de los dominios del bosque de Active Directory.  Este contenedor de objetos contendrá todos los objetos de dispositivo para el bosque de Active Directory.

De forma predeterminada, el contenedor se crea en el mismo dominio de AD FS.  \(Por ejemplo, **CN \= REGISTEREDDEVICES, DC \=<\- contexto de nomenclatura predeterminado \->** \) . Este objeto se crea cuando se inicializa el bosque de Active Directory para el registro de dispositivos.

### <a name="registered-devices"></a>Dispositivos registrados
Los objetos de dispositivo son objetos nuevos y ligeros de Active Directory.  Se usan para representar la relación entre: un usuario, un dispositivo y la empresa.  Los objetos de dispositivo usan un certificado firmado por AD FS para fijar el dispositivo físico en el objeto de dispositivo lógico en Active Directory.

Los dispositivos registrados incluyen los siguientes elementos:

-   **Nombre para mostrar**

    Nombre descriptivo del dispositivo.  Para dispositivos de Windows, este es el nombre de host del equipo.

-   **Identificador de dispositivo**

    Un GUID generado por el servidor de registro de dispositivos.

-   **Huella digital del certificado**

    La huella digital del certificado X.509 que se usa con el dispositivo registrado.

-   **OS Type** (Tipo de SO)

    Tipo de sistema operativo del dispositivo.

-   **Versión del SO.**

    La versión del sistema operativo en el dispositivo.

-   **Está habilitado**

    Valor booleano que indica si el dispositivo está habilitado en Active Directory.  Solamente se permite acceder a los servicios a los dispositivos habilitados.

-   **Hora aproximada del último uso**

    Hora aproximada a la que se usó el dispositivo para acceder a un recurso.  Para limitar el tráfico de replicación, esto solo se actualiza una vez cada 14 días.

-   **Propietario registrado**

    \(SID \) de identidad de seguridad del usuario que unió este dispositivo al área de trabajo.

## <a name="ad-fsdrs-server-ssl-certificate-revocation-checking"></a>AD FS \/ comprobación de revocación de certificados SSL del servidor DRS
El cliente de la Unión al área de trabajo comprueba la validez del certificado SSL del servidor de AD FS.  Si el certificado SSL de AD FS Server incluye un extremo de CRL de lista de revocación de certificados \( \) , el cliente debe poder alcanzar el extremo especificado para validar el certificado.

Si usa un entorno de prueba y una CA de entidad de certificación de prueba \( \) para emitir sus certificados SSL de servidor, puede optar por no incluir el extremo de CRL en los certificados de servidor emitidos por la CA.  Si lo hace permitirá que el cliente de la Unión al área de trabajo omita la comprobación CRL.

> [!CAUTION]
> Esto nunca se recomienda para los sistemas de producción

