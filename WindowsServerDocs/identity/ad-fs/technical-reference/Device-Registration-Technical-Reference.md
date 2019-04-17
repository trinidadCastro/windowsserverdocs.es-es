---
ms.assetid: 69ec592a-5499-4249-8ba0-afa356a8ff75
title: "Referencia técnica de registro de dispositivo"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: fac6437e9b6c3893064769a8279c2cf96cbc47d6
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2017
---
>Se aplica a: Windows Server 2016, Windows Server 2012 R2

# <a name="device-registration-technical-reference"></a>Referencia técnica de registro de dispositivo
El servicio de registro del dispositivo \(DRS\) es un nuevo servicio de Windows que se incluye con el rol de servicio de federación de Active Directory en Windows Server 2012 R2.  El DRS debe estar instalada y configurada en todos los servidores de federación de la batería de AD FS.  Para obtener información sobre cómo implementar DRS, vea [configurar un servidor de federación con el servicio de registro de dispositivo](https://technet.microsoft.com/library/dn486831.aspx).  
  
## <a name="active-directory-objects-created-when-a-device-is-registered"></a>Objetos de Active Directory creados al registra un dispositivo  
Se crean los siguientes objetos de Active Directory como parte del servicio de registro del dispositivo.  
  
### <a name="device-registration-configuration"></a>Configuración de registro del dispositivo  
La configuración de registro del dispositivo se almacena en el contexto de nomenclatura de configuración del bosque de Active Directory. \ (Por ejemplo, **CN\ = configuración de registro del dispositivo, CN\ = servicios, < contexto de naming\ configuration\ >**\). Este objeto se crea cuando se initialed bosque de Active Directory para el registro del dispositivo.  
  
La configuración de registro del dispositivo incluye los siguientes elementos:  
  
-   **Claves de emisor**  
  
    Las claves públicas y privadas utilizadas para emitir el certificado X.509 que está asociado con un dispositivo registrado.  Las claves privadas son DKM protegido.  
  
-   **Configuración del servicio de registro de dispositivo**  
  
    Directivas relacionados con el servicio de registro del dispositivo.  
  
### <a name="registered-devices-container"></a>Contenedor de dispositivos registrados  
El contenedor de objetos de dispositivo se crea en uno de los dominios del bosque de Active Directory.  Este contenedor de objetos contendrá todos los objetos de dispositivo para el bosque de Active Directory.  
  
De manera predeterminada, el contenedor se crea en el mismo dominio que AD FS.  \ (Por ejemplo, **CN\ = RegisteredDevices, DC\ = < contexto de naming\ default\ >**\). Este objeto se crea cuando se initialed bosque de Active Directory para el registro del dispositivo.  
  
### <a name="registered-devices"></a>Dispositivos registrados  
Objetos de dispositivo son nuevos y ligeros en Active Directory.  Se usa para representar la relación entre: un usuario, un dispositivo y la compañía.  Objetos de dispositivo usan un certificado firmado por AD FS para anclar el dispositivo físico para el objeto de dispositivo lógico en Active Directory.  
  
Los dispositivos registrados incluye los siguientes elementos:  
  
-   **Nombre para mostrar**  
  
    Nombre descriptivo del dispositivo.  Para los dispositivos de windows, esto es el nombre de host del equipo.  
  
-   **Identificador de dispositivo**  
  
    Un GUID que se genera por el servidor de registro del dispositivo.  
  
-   **Huella digital de certificado**  
  
    La huella digital de certificado del certificado X.509 que se usa con el dispositivo registrado.  
  
-   **Tipo de sistema operativo**  
  
    El tipo de sistema operativo en el dispositivo.  
  
-   **Versión del sistema operativo**  
  
    La versión del sistema operativo en el dispositivo.  
  
-   **Está habilitada**  
  
    Un valor booleano que indica si el dispositivo está habilitado en Active Directory.  Se permiten solo los dispositivos habilitados para tener acceso a los servicios.  
  
-   **Última vez que usa aproximada**  
  
    La hora aproximada en que el dispositivo se usa para acceder a un recurso.  Para limitar el tráfico de replicación, solo se actualiza una vez cada 14 días.  
  
-   **Propietario registrado**  
  
    La \(SID\) identidad de seguridad del usuario que ha unido a este dispositivo para el área de trabajo.  
  
## <a name="ad-fsdrs-server-ssl-certificate-revocation-checking"></a>Comprobación de revocación de certificados FS\ o DRS servidor SSL de anuncios  
El cliente al área de trabajo comprueba la validez del certificado de servidor SSL de AD FS.  Si el certificado SSL de servidor de AD FS incluye un extremo de la lista de revocación de certificados \(CRL\), el cliente debe ser capaz de alcanzar el extremo especificado para validar el certificado.  
  
Si estás usando un entorno de prueba y una entidad de certificación prueba \(CA\) para emitir el servidor de certificados SSL, a continuación, puedes elegir no incluir el extremo CRL en los certificados de servidor emitidos por la entidad de certificación.  Si lo haces, permitirá que el cliente al área de trabajo omitir la comprobación CRL.  
  
> [!CAUTION]  
> Esto nunca se recomienda para los sistemas de producción  
  

