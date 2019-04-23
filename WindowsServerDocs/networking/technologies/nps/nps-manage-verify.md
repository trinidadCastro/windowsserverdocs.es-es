---
title: Comprobar la configuración después de realizar cambios NPS
description: Puede utilizar este tema para comprobar la configuración de servidor de directivas de red de Windows Server 2016 después de cambiar una dirección IP o nombre para el servidor.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: fc77450e-2af1-47ba-bb23-1fd36d9efdbf
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 144e414e32d413e4863b90ada671753155bc96d7
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59880676"
---
# <a name="verify-configuration-after-nps-changes"></a>Comprobar la configuración después de realizar cambios NPS

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Puede utilizar este tema para comprobar la configuración de NPS después de cambiar una dirección IP o nombre para el servidor.

## <a name="verify-configuration-after-an-nps-ip-address-change"></a>Compruebe la configuración después de un cambio de dirección IP de NPS

Puede haber circunstancias donde tiene que cambiar la dirección IP de un proxy, como cuando mueve el servidor a una subred IP diferente o NPS. 

Si cambia una dirección IP de proxy o NPS, es necesario volver a configurar partes de la implementación de NPS. 

Utilice las siguientes directrices generales para ayudarle a comprobar que un cambio de dirección IP no interrumpe la autenticación de acceso de red, autorización o cuentas en la red para servidores NPS RADIUS y servidores proxy RADIUS.

Debe ser miembro del **administradores**, o equivalente, al realizar estos procedimientos.

### <a name="to-verify-configuration-after-an-nps-ip-address-change"></a>Para comprobar la configuración después de un cambio de dirección IP NPS

1. Volver a configurar a todos los clientes RADIUS, como los puntos de acceso inalámbrico y los servidores VPN, con la nueva dirección IP de NPS.

2. Si el NPS es miembro de un grupo de servidores RADIUS remotos, vuelva a configurar al proxy NPS con la nueva dirección IP de NPS.

3. Si ha configurado el NPS para usar el registro de SQL Server, compruebe que la conectividad entre el equipo que ejecuta SQL Server y el NPS sigue funcionando correctamente.

4. Si ha implementado IPsec para proteger el tráfico RADIUS entre el NPS y un proxy NPS u otros servidores o dispositivos, volver a configurar la directiva de IPsec o la regla de seguridad de conexión en el Firewall de Windows con seguridad avanzada para usar la nueva dirección IP de NPS.

5. Si el NPS es un host múltiple y ha configurado el servidor para enlazar a un adaptador de red específico, cambiar la configuración de puerto NPS con la nueva dirección IP.

### <a name="to-verify-configuration-after-an-nps-proxy-ip-address-change"></a>Para comprobar la configuración después de NPS de cambio de direcciones IP de proxy

1. Volver a configurar a todos los clientes RADIUS, como los puntos de acceso inalámbrico y los servidores VPN, con la nueva dirección IP del proxy NPS.

2. Si el proxy NPS es un host múltiple y ha configurado el servidor proxy para enlazar a un adaptador de red específico, cambiar la configuración de puerto NPS con la nueva dirección IP.

3. Volver a configurar a todos los miembros de todos los grupos de servidores RADIUS remotos con la dirección IP del servidor proxy. Para realizar esta tarea, en cada NPS con el proxy NPS configurado como cliente RADIUS:

    a. Haga doble clic en **NPS (Local)**, haga doble clic en **clientes y servidores RADIUS**, haga clic en **clientes RADIUS**y, a continuación, en el panel de detalles, haga doble clic en el cliente RADIUS que ¿desea cambiar.

    b. En el cliente RADIUS **propiedades**, en **dirección \(IP o DNS\)**, escriba la nueva dirección IP del proxy NPS.

4. Si ha configurado el proxy NPS para usar el registro de SQL Server, compruebe que la conectividad entre el equipo que ejecuta SQL Server y el proxy NPS sigue funcionando correctamente.

## <a name="verify-configuration-after-renaming-an-nps"></a>Compruebe la configuración después de cambiar el nombre de NPS

Puede haber circunstancias cuando deba cambiar el nombre de un proxy, por ejemplo, al diseñar las convenciones de nomenclatura para los servidores o NPS.

Si cambia el nombre de un proxy o NPS, es necesario volver a configurar partes de la implementación de NPS. 

Utilice las siguientes directrices generales para ayudarle a comprobar que un cambio de nombre de servidor no interrumpe las cuentas, autorización o autenticación de acceso de red.

Debe ser miembro del **administradores**, o equivalente, al llevar a cabo este procedimiento.

### <a name="to-verify-configuration-after-an-nps-or-proxy-name-change"></a>Para comprobar la configuración después de un cambio de nombre de NPS o proxy

1. Si el NPS es miembro de un grupo de servidores RADIUS remotos y el grupo está configurado con los nombres de equipo en lugar de direcciones IP, volver a configurar el grupo de servidores RADIUS remotos con el nuevo nombre NPS.

2. Si se implementan los métodos de autenticación basada en certificados en el NPS, el cambio de nombre invalida el certificado de servidor. Puede solicitar un nuevo certificado desde el Administrador de certificación emisora (CA) o, si el equipo es un equipo miembro del dominio y que los certificados de inscripción automática a los miembros de dominio, puede actualizar la directiva de grupo para obtener un certificado nuevo a través de la inscripción automática . Para actualizar la directiva de grupo:

    a. Abra el símbolo del sistema o Windows PowerShell.

    b. Escriba **gpupdate** y presione ENTRAR.


3. Una vez que un nuevo certificado de servidor, solicitar que el Administrador de CA revocar el certificado antiguo. 

     Una vez que se revoca el certificado antiguo, NPS continúa usando hasta que expire el certificado antiguo. De forma predeterminada, el certificado antiguo sigue siendo válido durante un tiempo máximo de una semana y 10 horas. Este período de tiempo puede ser diferente dependiendo de si la expiración de la lista de revocación de certificados (CRL) y la caducidad de tiempo de caché de capa de transporte (TLS) se han modificado de sus valores predeterminados. La expiración de la CRL predeterminado es una semana; tiempo de expiración es de 10 horas en caché el valor predeterminado TLS. 

     Sin embargo, si desea configurar NPS para usar el nuevo certificado de forma inmediata, puede reconfigurar manualmente las directivas de red con el nuevo certificado.

4. Después de que expire el certificado antiguo, NPS comienza automáticamente con el nuevo certificado. 

5. Si ha configurado el NPS para usar el registro de SQL Server, compruebe que la conectividad entre el equipo que ejecuta SQL Server y el NPS sigue funcionando correctamente.

