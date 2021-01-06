---
title: Configurar la información del puerto UDP de NPS
description: Puede usar este tema para configurar los puertos que usa el servidor de directivas de redes (NPS) para el tráfico de autenticación y cuentas de Servicio de autenticación remota telefónica de usuario (RADIUS) en Windows Server 2016.
manager: brianlic
ms.topic: article
ms.assetid: 70569958-d7a7-474e-a817-6b7b5134784a
ms.author: lizross
author: eross-msft
ms.date: 08/07/2020
ms.openlocfilehash: 94a8e67056dff497f1adc18d898a6c87f4ab665c
ms.sourcegitcommit: 40905b1f9d68f1b7d821e05cab2d35e9b425e38d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/06/2021
ms.locfileid: "97947441"
---
# <a name="configure-nps-udp-port-information"></a>Configurar la información del puerto UDP de NPS

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Puede usar el siguiente procedimiento para configurar los puertos que usa el servidor de directivas de redes (NPS) para Servicio de autenticación remota telefónica de usuario el \( \) tráfico de autenticación y cuentas de RADIUS.

De forma predeterminada, NPS escucha el tráfico RADIUS en los puertos 1812, 1813, 1645 y 1646 para el protocolo de Internet versión 6 \( IPv6 \) e IPv4 para todos los adaptadores de red instalados.

>[!NOTE]
>Si desinstala IPv4 o IPv6 en un adaptador de red, NPS no supervisará el tráfico RADIUS para el protocolo desinstalado.

Los valores de puerto de 1812 para la autenticación y 1813 para cuentas son puertos RADIUS estándar definidos por el equipo de ingeniería de Internet ( \( IETF) \) en RFC 2865 y 2866. Sin embargo, de forma predeterminada, muchos servidores de acceso usan los puertos 1645 para las solicitudes de autenticación y 1646 para las solicitudes de cuentas. Con independencia de los números de puerto que decida usar, asegúrese de que NPS y el servidor de acceso estén configurados para usarlos.

>AÚN Si no usa los números de puerto predeterminados de RADIUS, debe configurar excepciones en el firewall para que el equipo local permita el tráfico RADIUS en los nuevos puertos. Para obtener más información, consulte [configurar firewalls para el tráfico RADIUS](nps-firewalls-configure.md).

La pertenencia a **Administradores de dominio**, o equivalente, es lo mínimo necesario para completar este procedimiento.

## <a name="to-configure-nps-udp-port-information"></a>Para configurar la información del puerto UDP de NPS

1. Abra la consola de NPS.
2. Haga clic con el botón secundario en **servidor de directivas de redes** y, a continuación, haga clic en **propiedades**.
3. Haga clic en la pestaña **puertos** y examine la configuración de puertos. Si los puertos UDP de autenticación y cuentas RADIUS son distintos de los valores predeterminados proporcionados (1812 y 1645 para la autenticación, y 1813 y 1646 para cuentas), escriba la configuración del puerto en **autenticación** y **cuentas**.
4. Para usar varios valores de puertos para las solicitudes de autenticación y cuentas, separe los números de puerto con comas.

Para obtener más información sobre la administración de NPS, consulte [administrar el servidor de directivas de redes](nps-manage-top.md).

Para obtener más información acerca de NPS, consulte [servidor de directivas de redes (NPS)](nps-top.md).
