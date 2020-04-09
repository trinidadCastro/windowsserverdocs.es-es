---
title: Solucionar problemas relacionados con la activación de multisitio
description: Este tema forma parte de la guía de implementación de varios servidores de acceso remoto en una implementación multisitio en Windows Server 2016.
manager: brianlic
ms.prod: windows-server
ms.technology: networking-ras
ms.topic: article
ms.assetid: 570c81d6-c4f4-464c-bee9-0acbd4993584
ms.author: lizross
author: eross-msft
ms.openlocfilehash: ae01a63a494504120ca248ec56dd9bc9648c1ae4
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80858298"
---
# <a name="troubleshooting-enabling-multisite"></a>Solucionar problemas relacionados con la activación de multisitio

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

En este tema encontrará información para solucionar problemas relacionados con el comando `Enable-DAMultisite`. Para confirmar que el error recibido está relacionado con la activación de la funcionalidad de multisitio, consulte el identificador de evento 10051 en el Registro de eventos de Windows.  
  
## <a name="user-connectivity-issues"></a>Problemas de conectividad del usuario  
Los usuarios pueden enfrentarse a problemas si la funcionalidad de multisitio está habilitada y la configuración no es la adecuada.  
  
**Causa**  
  
En una implementación multisitio, los equipos cliente de Windows 10 y Windows 8 pueden moverse entre distintos puntos de entrada.  Los equipos cliente de Windows 7 deben estar asociados a un punto de entrada específico en la implementación multisitio. Por lo tanto, si los equipos cliente no se encuentran en el grupo de seguridad apropiado, existe la posibilidad de que se les aplique la configuración de directiva de grupo incorrecta.  
  
**Solución**  
  
DirectAccess requiere al menos un grupo de seguridad para todos los equipos cliente de Windows 10 y Windows 8. se recomienda usar un grupo de seguridad para todos los equipos con Windows 10 y Windows 8 por dominio. DirectAccess también necesita un grupo de seguridad para equipos cliente de Windows 7 para cada punto de entrada. Cada equipo cliente debe estar en un solo grupo de seguridad. Por lo tanto, debe asegurarse de que los grupos de seguridad para los clientes de Windows 10 y Windows 8 contienen solo equipos que ejecutan Windows 10 o Windows 8 y que cada equipo cliente de Windows 7 pertenece a un solo grupo de seguridad dedicado para el punto de entrada correspondiente. que ningún cliente de Windows 10 o Windows 8 pertenezca a los grupos de seguridad de Windows 7.  
  
Configure los grupos de seguridad de Windows 8 en la página **seleccionar grupos** del Asistente para la **instalación del cliente de DirectAccess** . Configure los grupos de seguridad de Windows 7 en la página **compatibilidad con clientes** del Asistente para **Habilitar la implementación multisitio** o en la página **compatibilidad con clientes** del Asistente para **Agregar un punto de entrada** .  
  
## <a name="kerberos-proxy-authentication"></a>Autenticación proxy de Kerberos  
**Error recibido**. La autenticación de proxy Kerberos no se admite en una implementación multisitio. Debe habilitar el uso de certificados de equipo para la autenticación de usuarios IPsec.  
  
**Causa**  
  
La autenticación de certificados de equipo debe estar habilitada para poder habilitar la funcionalidad de multisitio.  
  
**Solución**  
  
Para habilitar la autenticación de certificados de equipo  
  
1.  En el área **Paso 2: Servidor de acceso remoto** del panel de detalles de la Consola de administración de acceso remoto, haga clic en **Editar**.  
  
2.  En la página **Autenticación** del **Asistente para la instalación del servidor de acceso remoto**, active la casilla **Usar certificados de equipo** y seleccione la entidad de certificación raíz o intermedia que emite certificados en su implementación.  
  
Para habilitar la autenticación de certificados de equipo mediante Windows PowerShell, use el cmdlet `Set-DAServer` y especifique el parámetro *IPsecRootCertificate* .  
  
## <a name="ip-https-certificates"></a>Certificados IP-HTTPS  
**Error recibido**. El servidor de DirectAccess usa un certificado IP-HTTPS autofirmado. Configure IP-HTTPS para que use un certificado firmado de una CA conocida.  
  
**Causa**  
  
El certificado IP-HTTPS es autofirmado. Este tipo de certificados no se puede usar en una implementación multisitio.  
  
**Solución**  
  
Para seleccionar un certificado IP-HTTPS:  
  
1.  En el área **Paso 2: Servidor de acceso remoto** del panel de detalles de la Consola de administración de acceso remoto, haga clic en **Editar**.  
  
2.  En la página **Adaptadores de red** del **Asistente para la instalación del servidor de acceso remoto**, asegúrese de que la casilla **Usar un certificado autofirmado creado automáticamente por DirectAccess** en **Seleccione el certificado usado para autenticar conexiones IP-HTTPS** está desactivada y, después, haga clic en **Examinar** para seleccionar un certificado emitido por una CA de confianza.  
  
## <a name="network-location-server"></a>Servidor de ubicación de red  
  
-   **Problema 1**  
  
    **Error recibido**. DirectAccess está configurado para usar un certificado autofirmado para el servidor de ubicación de red. Configure el servidor de ubicación de red para que use un certificado firmado por una entidad de certificación.  
  
    **Causa**  
  
    El servidor de ubicación de red se implementa en el servidor de acceso remoto y emplea un certificado autofirmado. Este tipo de certificados no se puede usar en una implementación multisitio.  
  
    **Solución**  
  
    Para seleccionar un certificado de servidor de ubicación de red:  
  
    1.  En el área **Paso 3: Servidores de infraestructura** del panel de detalles de la Consola de administración de acceso remoto, haga clic en **Editar**.  
  
    2.  En la página **Servidor de ubicación de red** del **Asistente para la instalación del servidor de infraestructura**, asegúrese de que la casilla **Usar un certificado autofirmado** en **El servidor de ubicación de red se implementa en el servidor de acceso remoto** está desactivada y, después, haga clic en **Examinar** para seleccionar un certificado emitido por una CA empresarial.  
  
-   **Problema 2**  
  
    **Error recibido**. Para implementar un clúster con equilibrio de carga de red o una implementación multisitio, obtenga un certificado para el servidor de ubicación de red con un nombre de firmante distinto del nombre interno del servidor de acceso remoto.  
  
    **Causa**  
  
    El nombre de firmante del certificado que se ha usado para el sitio web del servidor de ubicación de red es el mismo que el nombre interno del servidor de acceso remoto. Esto hará que surjan problemas de resolución de nombres.  
  
    **Solución**  
  
    Obtenga un certificado con un nombre de firmante distinto del nombre interno del servidor de acceso remoto.  
  
    Para configurar el servidor de ubicación de red:  
  
    1.  En el área **Paso 3: Servidores de infraestructura** del panel de detalles de la Consola de administración de acceso remoto, haga clic en **Editar**.  
  
    2.  En la página **Servidor de ubicación de red** del **Asistente para la instalación del servidor de infraestructura**, haga clic en **Examinar** en **El servidor de ubicación de red se implementa en el servidor de acceso remoto** para seleccionar el certificado obtenido previamente. Este certificado debe tener un nombre de firmante distinto del nombre interno del servidor de acceso remoto.  
  
## <a name="windows-7-client-computers"></a>Equipos cliente de Windows 7  
**ADVERTENCIA recibida**. Al habilitar multisitio, los grupos de seguridad configurados para los clientes de DirectAccess no deben contener equipos con Windows 7. Para admitir equipos cliente de Windows 7 en una implementación multisitio, seleccione un grupo de seguridad que contenga los clientes de cada punto de entrada.  
  
**Causa**  
  
En la implementación de DirectAccess existente, se habilitó la compatibilidad con clientes de Windows 7.  
  
**Solución**  
  
DirectAccess requiere al menos un grupo de seguridad para todos los equipos cliente de Windows 8 y un grupo de seguridad para equipos cliente de Windows 7 para cada punto de entrada. Cada equipo cliente debe estar en un solo grupo de seguridad. Por lo tanto, debe asegurarse de que el grupo de seguridad para clientes de Windows 8 solo contiene equipos que ejecutan Windows 8 y que cada equipo cliente de Windows 7 pertenece a un solo grupo de seguridad dedicado para el punto de entrada relevante y que no hay clientes de Windows 8. pertenecer a los grupos de seguridad de Windows 7.  
  
## <a name="active-directory-site"></a>Sitio de Active Directory  
**Error recibido**. El > de server_name del servidor < no está asociado a un sitio Active Directory.  
  
**Causa**  
  
DirectAccess no pudo determinar el sitio de Active Directory. En la consola Sitios y servicios de Active Directory se pueden configurar subredes diferentes para la red y asociar cada una de ellas con el sitio de Active Directory que proceda. Este error puede aparecer si la dirección IP del servidor de acceso remoto no pertenece a ninguna de las subredes o si la subred a la que la dirección IP pertenece no se ha definido con un sitio de Active Directory.  
  
**Solución**  
  
Confirme que se trata de este problema; para ello, ejecute el comando `nltest /dsgetsite` en el servidor de acceso remoto. Si lo es, el comando devolverá ERROR_NO_SITENAME. Para solucionarlo, en el controlador de dominio, asegúrese de que existe una subred que contiene la dirección IP de servidor interna y de que está definida con un sitio de Active Directory.  
  
## <a name="saving-server-gpo-settings"></a><a name="SaveGPOSettings"></a>Guardando configuración de GPO de servidor  
**Error recibido**. Error al guardar la configuración de acceso remoto en el GPO < GPO_name >.  
  
**Causa**  
  
No se pudieron guardar los cambios en el GPO de servidor debido a problemas de conectividad o a una infracción de uso compartido en el archivo Registry. Pol; por ejemplo, otro usuario ha bloqueado el archivo.  
  
**Solución**  
  
Asegúrese de que hay conectividad entre el servidor de acceso remoto y el controlador de dominio. Si la hay, compruebe en el controlador de dominio si otro usuario ha bloqueado el archivo registry.pol y, en caso necesario, finalice la sesión de dicho usuario para desbloquearlo.  
  
## <a name="internal-error-occurred"></a><a name="InternalServerError"></a>Se produjo un error interno  
**Error recibido**. Error interno.  
  
**Causa**  
  
Esto puede deberse a una configuración inesperada de la tabla de puntos de entrada en el GPO de cliente. Puede suceder cuando el administrador usa cmdlets de cliente de DirectAccess para modificar la tabla de puntos de entrada en el GPO de cliente.  
  
**Solución**  
  
Repase la configuración de la tabla de puntos de entrada en todos los GPO de cliente y arregle las incoherencias que pueda haber en la configuración multisitio entre las distintas instancias de los GPO de cliente y la configuración de DirectAccess. Use el cmdlet `Get-DaEntryPointTableItem` con el nombre del GPO de cliente para obtener la tabla de puntos de entrada relativa al cliente. Use el cmdlet `Get-NetIPHttpsConfiguration` para obtener todos los perfiles IP-HTTPS de todos los puntos de entrada.  
  
Para obtener más información, vea [cmdlets de cliente de DirectAccess en Windows PowerShell](https://technet.microsoft.com/library/hh848426).  
  


