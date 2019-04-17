---
title: Comprueba la configuración después de cambios en el servidor NPS
description: Puedes usar este tema para comprobar la configuración del servidor de directivas de red de Windows Server 2016 después de cambiar una dirección IP o el nombre en el servidor.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: fc77450e-2af1-47ba-bb23-1fd36d9efdbf
ms.author: pashort
author: shortpatti
ms.openlocfilehash: f4d7e003fb037d18c5e5f2036a419383885eaf9e
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2018
---
# <a name="verify-configuration-after-nps-server-changes"></a>Comprueba la configuración después de cambios en el servidor NPS

>Se aplica a: Windows Server (punto y anual canal), Windows Server 2016

Puedes usar este tema para comprobar la configuración del servidor NPS después de cambiar una dirección IP o el nombre en el servidor.

## <a name="verify-configuration-after-an-nps-server-ip-address-change"></a>Comprueba la configuración después de un cambio de dirección IP de servidor NPS

Pueden darse circunstancias donde necesites cambiar la dirección IP de un servidor NPS o el servidor proxy, como cuando mueves el servidor a una subred IP diferente. 

Si cambias un servidor NPS o la dirección IP del proxy, es necesario volver a configurar las partes de la implementación de NPS. 

Usa las siguientes directrices generales para ayudarle a comprobar que cambia la dirección IP no interrumpe la autenticación de acceso de red, autorización o cuentas de la red para los servidores RADIUS NPS y servidores proxy RADIUS.

Debe ser miembro del **administradores**, o equivalente, para realizar estos procedimientos.

### <a name="to-verify-configuration-after-an-nps-server-ip-address-change"></a>Para comprobar la configuración de NPS después de la dirección IP del servidor dirección cambiar

1. Volver a configurar a todos los clientes RADIUS, como los puntos de acceso inalámbrico y servidores VPN, con la nueva dirección IP del servidor NPS.

2. Si el servidor NPS es un miembro de un grupo de servidores RADIUS remotos, volver a configurar al proxy NPS con la nueva dirección IP del servidor NPS.

3. Si has configurado el servidor NPS para usar el registro de SQL Server, compruebe que la conectividad entre el equipo que ejecuta SQL Server y el servidor NPS sigue funcionando correctamente.

4. Si ha implementado IPsec para proteger el tráfico entre el servidor NPS y un proxy NPS u otros servidores o dispositivos, volver a configurar la directiva IPsec o la regla de seguridad de conexión en Firewall de Windows con seguridad avanzada para usar la nueva dirección IP del servidor NPS.

5. Si el servidor NPS es múltiple y que has configurado el servidor para enlazar a un adaptador de red específica, reconfigurar NPS puerto con la nueva dirección IP.

### <a name="to-verify-configuration-after-an-nps-proxy-ip-address-change"></a>Para comprobar la configuración después de NPS de cambio de direcciones IP de proxy

1. Volver a configurar a todos los clientes RADIUS, como los puntos de acceso inalámbrico y servidores VPN, con la nueva dirección IP del servidor proxy NPS.

2. Si el proxy NPS es múltiple y has configurado el proxy para enlazar a un adaptador de red específica, reconfigurar NPS puerto con la nueva dirección IP.

3. Volver a configurar a todos los miembros de todos los grupos de servidores RADIUS remotos con la dirección IP del servidor proxy. Para realizar esta tarea, en cada servidor NPS que tiene el proxy NPS configurado como un cliente RADIUS:

    Un archivo. Haz doble clic en **NPS (Local)**, haz doble clic en **clientes y servidores RADIUS**, haz clic en **clientes RADIUS**y, a continuación, en el panel de detalles, haz doble clic en el cliente RADIUS que desea cambiar.

    b. En el cliente RADIUS **propiedades**, en **dirección \(IP or DNS\)**, escribe la nueva dirección IP del servidor proxy NPS.

4. Si has configurado el proxy NPS para usar el registro de SQL Server, compruebe que la conectividad entre el equipo que ejecuta SQL Server y el proxy NPS sigue funcionando correctamente.

## <a name="verify-configuration-after-renaming-an-nps-server"></a>Comprueba la configuración después de cambiar el nombre de un servidor NPS

Cuando se necesita cambiar el nombre de un servidor NPS o el servidor proxy, como cuando se vuelva a diseñar las convenciones de nomenclatura para los servidores, pueden darse circunstancias.

Si cambias un nombre de proxy o servidor NPS, es necesario volver a configurar las partes de la implementación de NPS. 

Usa las siguientes directrices generales para ayudarle a comprobar que un cambio de nombre de servidor no interrumpe la autenticación de acceso de red, autorización o contabilidad.

Debe ser miembro del **administradores**, o equivalente, para realizar este procedimiento.

### <a name="to-verify-configuration-after-an-nps-server-or-proxy-name-change"></a>Para comprobar la configuración después de un cambio de nombre del proxy o servidor NPS

1. Si el servidor NPS es un miembro de un grupo de servidores RADIUS remotos, y el grupo está configurado con nombres de equipo en lugar de direcciones IP, volver a configurar el grupo de servidores RADIUS remoto con el nuevo nombre del servidor NPS.

2. Si se implementan los métodos de autenticación basada en certificados en el servidor NPS, el cambio de nombre invalida el certificado de servidor. Puedes solicitar un nuevo certificado desde el Administrador de la entidad de (certificación CA) de certificación, o bien, si el equipo está un equipo miembro del dominio e inscribir automáticamente los certificados a los miembros de dominio, puede actualizar la directiva de grupo para obtener un nuevo certificado a través de la inscripción automática. Para actualizar la directiva de grupo:

    Un archivo. Abre Windows PowerShell o el símbolo del sistema.

    b. Tipo **gpupdate**, y, a continuación, presione ENTRAR.


3. Una vez que tengas un nuevo certificado de servidor, solicitar que el Administrador de CA revocar el antiguo certificado. 

     Una vez se revoca el antiguo certificado NPS continúa usarlo hasta que expira el antiguo certificado. De manera predeterminada, el antiguo certificado sigue siendo válido durante un tiempo máximo de una semana y 10 horas. Este período de tiempo puede ser diferente dependiendo de si la expiración de la lista de revocación de certificados (CRL) y la expiración de tiempo de caché de seguridad de la capa de transporte (TLS) se modificaron sus valores predeterminados. La expiración CRL predeterminado es una semana; el valor predeterminado TLS caché tiempo transcurrido es 10 horas. 

     Sin embargo, si quieres configurar NPS para usar el nuevo certificado inmediatamente, puedes configurar manualmente las directivas de red con el nuevo certificado.

4. Cuando expira el antiguo certificado, NPS comienza automáticamente con el nuevo certificado. 

5. Si has configurado el servidor NPS para usar el registro de SQL Server, compruebe que la conectividad entre el equipo que ejecuta SQL Server y el servidor NPS sigue funcionando correctamente.

