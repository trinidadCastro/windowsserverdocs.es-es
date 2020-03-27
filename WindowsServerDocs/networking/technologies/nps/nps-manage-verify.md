---
title: Comprobar la configuración después de que NPS cambie
description: Puede usar este tema para comprobar la configuración del servidor de directivas de redes de Windows Server 2016 después de cambiar la dirección IP o el nombre al servidor.
manager: brianlic
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: fc77450e-2af1-47ba-bb23-1fd36d9efdbf
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 5a99cfb62d3ce331ef2d90fe13afe99a6d14960c
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/26/2020
ms.locfileid: "80315886"
---
# <a name="verify-configuration-after-nps-changes"></a>Comprobar la configuración después de que NPS cambie

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Puede usar este tema para comprobar la configuración de NPS después de un cambio de nombre o dirección IP en el servidor.

## <a name="verify-configuration-after-an-nps-ip-address-change"></a>Comprobar la configuración después de un cambio de dirección IP de NPS

Puede haber casos en los que necesite cambiar la dirección IP de un servidor NPS o proxy, por ejemplo, cuando mueva el servidor a una subred IP diferente. 

Si cambia una dirección IP de NPS o proxy, es necesario volver a configurar partes de la implementación de NPS. 

Use las siguientes directrices generales para ayudarle a comprobar que un cambio de dirección IP no interrumpa la autenticación, autorización o cuentas de acceso a la red en la red para los servidores RADIUS NPS y los servidores proxy RADIUS.

Para realizar estos procedimientos, debe ser miembro del **grupo administradores**o equivalente.

### <a name="to-verify-configuration-after-an-nps-ip-address-change"></a>Para comprobar la configuración después de un cambio de dirección IP de NPS

1. Vuelva a configurar todos los clientes RADIUS, como los puntos de acceso inalámbricos y los servidores VPN, con la nueva dirección IP del NPS.

2. Si el NPS es miembro de un grupo de servidores RADIUS remotos, vuelva a configurar el proxy NPS con la nueva dirección IP del NPS.

3. Si ha configurado el NPS para usar el registro de SQL Server, compruebe que la conectividad entre el equipo que ejecuta SQL Server y el NPS siga funcionando correctamente.

4. Si ha implementado IPsec para proteger el tráfico RADIUS entre el NPS y un proxy NPS u otros servidores o dispositivos, vuelva a configurar la directiva IPsec o la regla de seguridad de conexión en firewall de Windows con seguridad avanzada para usar la nueva dirección IP del NPS.

5. Si el NPS es de host múltiple y ha configurado el servidor para enlazar a un adaptador de red específico, vuelva a configurar la configuración del puerto NPS con la nueva dirección IP.

### <a name="to-verify-configuration-after-an-nps-proxy-ip-address-change"></a>Para comprobar la configuración después de un cambio de dirección IP del proxy NPS

1. Vuelva a configurar todos los clientes RADIUS, como los puntos de acceso inalámbricos y los servidores VPN, con la nueva dirección IP del proxy NPS.

2. Si el proxy NPS es de host múltiple y ha configurado el proxy para enlazar a un adaptador de red específico, vuelva a configurar la configuración del puerto NPS con la nueva dirección IP.

3. Volver a configurar todos los miembros de todos los grupos de servidores RADIUS remotos con la dirección IP del servidor proxy. Para realizar esta tarea, en cada NPS que tenga el proxy NPS configurado como cliente RADIUS:

    a. Haga doble clic en **NPS (local)** , haga doble clic en **clientes y servidores RADIUS**, haga clic en **clientes RADIUS**y, a continuación, en el panel de detalles, haga doble clic en el cliente RADIUS que desee cambiar.

    b. En **propiedades**del cliente RADIUS, en **Dirección \(IP o\)DNS** , escriba la nueva dirección IP del proxy NPS.

4. Si ha configurado el proxy NPS para usar el registro de SQL Server, compruebe que la conectividad entre el equipo que ejecuta SQL Server y el proxy NPS siga funcionando correctamente.

## <a name="verify-configuration-after-renaming-an-nps"></a>Comprobar la configuración después de cambiar el nombre de un NPS

Puede haber circunstancias en las que necesite cambiar el nombre de un servidor NPS o proxy, por ejemplo, al rediseñar las convenciones de nomenclatura para los servidores.

Si cambia un nombre de servidor proxy o NPS, es necesario volver a configurar partes de la implementación de NPS. 

Utilice las siguientes directrices generales para ayudarle a comprobar que un cambio de nombre de servidor no interrumpa la autenticación, autorización o cuentas de acceso a la red.

Para llevar a cabo este procedimiento, debe ser miembro del **grupo administradores**o equivalente.

### <a name="to-verify-configuration-after-an-nps-or-proxy-name-change"></a>Para comprobar la configuración después de un cambio de nombre de proxy o NPS

1. Si el NPS es miembro de un grupo de servidores RADIUS remotos y el grupo está configurado con nombres de equipo en lugar de direcciones IP, vuelva a configurar el grupo de servidores remotos RADIUS con el nuevo nombre NPS.

2. Si se implementan métodos de autenticación basados en certificados en el NPS, el cambio de nombre invalida el certificado de servidor. Puede solicitar un nuevo certificado desde el administrador de la entidad de certificación (CA) o, si el equipo es miembro de un dominio y realizar la inscripción automática de certificados en los miembros del dominio, puede actualizar directiva de grupo para obtener un certificado nuevo a través de la inscripción automática. . Para actualizar directiva de grupo:

    a. Abra el símbolo del sistema o Windows PowerShell.

    b. Escriba **gpupdate** y presione ENTRAR.


3. Una vez que tenga un nuevo certificado de servidor, solicite que el administrador de CA revoque el certificado antiguo. 

     Una vez revocado el certificado anterior, NPS lo seguirá usando hasta que expire el certificado anterior. De forma predeterminada, el certificado antiguo sigue siendo válido durante un tiempo máximo de una semana y 10 horas. Este período de tiempo puede variar en función de si la expiración de la lista de revocación de certificados (CRL) y la expiración del tiempo de la caché de seguridad de la capa de transporte (TLS) se han modificado con respecto a sus valores predeterminados. La expiración de CRL predeterminada es de una semana; la expiración predeterminada del tiempo de caché TLS es de 10 horas. 

     No obstante, si desea configurar NPS para que use el nuevo certificado de forma inmediata, puede volver a configurar manualmente las directivas de red con el nuevo certificado.

4. Después de que expire el certificado anterior, NPS comienza a usar automáticamente el nuevo certificado. 

5. Si ha configurado el NPS para usar el registro de SQL Server, compruebe que la conectividad entre el equipo que ejecuta SQL Server y el NPS siga funcionando correctamente.

