---
title: Configurar la información de puerto UDP de NPS
description: Puedes usar este tema para configurar los puertos que usa el servidor de directivas de redes (NPS) para el servicio de autenticación remota telefónica de usuario (RADIUS) y el tráfico de cuentas en Windows Server 2016.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 70569958-d7a7-474e-a817-6b7b5134784a
ms.author: pashort
author: shortpatti
ms.openlocfilehash: f0e703dc6f9083f1e79091a6cee6d1ac58753d12
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2018
---
# <a name="configure-nps-udp-port-information"></a>Configurar la información de puerto UDP de NPS

>Se aplica a: Windows Server (punto y anual canal), Windows Server 2016

Puedes usar el siguiente procedimiento para configurar los puertos que usa el servidor de directivas de redes (NPS) para la autenticación de \(RADIUS\) servicio autenticación remota telefónica de usuario y el tráfico de contabilidad.

De manera predeterminada, NPS escucha el tráfico RADIUS en puertos 1812, 1813, 1645 y 1646 para el protocolo de Internet versión 6 \(IPv6\) e IPv4 para todos los adaptadores de red instalados.

>[!NOTE]
>Si desinstala IPv4 o IPv6 en un adaptador de red, NPS no supervisa el tráfico RADIUS para el protocolo desinstalado.

Los valores de puerto de 1812 para la autenticación y 1813 para cuentas son puertos estándar RADIUS definidos por el \(IETF\) grupo de trabajo de ingeniería de Internet en RFC 2865 y 2866. Sin embargo, de manera predeterminada, muchos servidores de acceso usan puertos 1645 para solicitudes de autenticación y 1646 para las solicitudes de contabilidad. Independientemente de los números de puerto que decida usar, asegúrese de que NPS y el servidor de acceso están configurados para usar los mismos.

>[IMPORTANTE] Si no usas los números de puerto de radio de forma predeterminada, debes configurar excepciones en el firewall para el equipo local permitir el tráfico de radio en los puertos nuevo. Para obtener más información, consulta [configurar el firewall para el tráfico RADIUS](nps-firewalls-configure.md).

Pertenencia a **administradores de dominio**, o equivalente, es lo mínimo necesario para completar este procedimiento.

## <a name="to-configure-nps-udp-port-information"></a>Para configurar la información de puerto UDP de NPS 

1. Abre la consola NPS.
2. Haz clic en **el servidor de directivas de red**y, a continuación, haz clic en **propiedades**.
3. Haz clic en el **puertos** pestaña y, a continuación, examine la configuración de puertos. Si la autenticación RADIUS RADIUS puertos UDP y cuentas varían desde los valores predeterminados proporcionados (1812 y 1645 para la autenticación y 1813 y 1646 para cuentas), escribe la configuración del puerto en **autenticación** y **contabilidad**.
4. Para usar varias configuraciones de puerto para la autenticación o las solicitudes de cuentas, separe los números de puerto con comas.

Para obtener más información acerca de cómo administrar NPS, consulta [administrar el servidor de directivas de red](nps-manage-top.md).

Para obtener más información acerca de NPS, consulta [servidor de directivas de redes (NPS)](nps-top.md).
