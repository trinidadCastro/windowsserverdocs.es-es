---
title: Cómo funciona la transferencia en el servicio de migración de almacenamiento
description: Resumen y detalles de las fases de traslado en el servicio de migración de almacenamiento
author: nedpyle
ms.author: nedpyle
ms.date: 08/31/2020
ms.topic: article
ms.openlocfilehash: 2f323b03052f0a920ff064860bf5cbf4c6a2f01e
ms.sourcegitcommit: 014e730c7fb19ed06393d144a446e4248fc35444
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/29/2020
ms.locfileid: "91495652"
---
# <a name="how-cutover-works-in-storage-migration-service"></a>Cómo funciona la transferencia en el servicio de migración de almacenamiento

La transferencia es la fase de la migración que mueve la identidad de red del equipo de origen al equipo de destino. Después del traslado, el equipo de origen seguirá conteniendo los mismos archivos que antes, pero no estará disponible para los usuarios y las aplicaciones.

## <a name="summary"></a>Resumen

![Captura de pantalla de configuración de traslado ](media/cutover/cutover_configuration.png)
 __figura 1: configuración de traslado de servicio de migración de almacenamiento__

Antes de que se inicie la transferencia, se proporciona la información de configuración de red necesaria para pasar del equipo de origen al equipo de destino. También puede elegir un nuevo nombre único para el equipo de origen o permitir que el servicio de migración de almacenamiento cree uno aleatorio.

Después, el servicio de migración de almacenamiento realiza los pasos siguientes para cortar el equipo de origen en el equipo de destino:

1. Nos conectamos a los equipos de origen y de destino. Ya deben tener las siguientes reglas de Firewall habilitadas como entrantes:
    * Compartir archivos e impresoras (SMB-in), puerto TCP 445
    * Servicio NetLogon (NP-in), puerto TCP 445
    * Instrumental de administración de Windows (DCOM-in), puerto TCP 135
    * Instrumental de administración de Windows (WMI-in), TCP, cualquier puerto

2. Establecemos permisos de seguridad en el equipo de destino en Active Directory Domain Services para que coincidan con los permisos del equipo de origen.

3. Creamos una cuenta de usuario local temporal en el equipo de origen. Si el equipo está unido a un dominio, el nombre de usuario de la cuenta es "MsftSmsStorMigratSvc". Deshabilitaremos la [Directiva de filtro de tokens de cuenta local](https://support.microsoft.com/help/951016/description-of-user-account-control-and-remote-restrictions-in-windows) en el equipo de origen para permitir la cuenta a través de y, a continuación, conectarse al equipo de origen. Hacemos esta cuenta temporal para que, cuando reiniciemos y quitemos el equipo de origen del dominio más adelante, todavía podamos tener acceso al equipo de origen.

4. Repetimos el paso anterior en el equipo de destino.

5. Quitamos el equipo de origen del dominio para liberar su Active Directory cuenta, que el equipo de destino tomará más adelante.

6. Asignamos interfaces de red en el equipo de origen y cambiamos el nombre del equipo de origen.

7. El equipo de origen se vuelve a agregar al dominio. El equipo de origen tiene ahora una nueva identidad y está disponible para los administradores, pero no para los usuarios y las aplicaciones.

8. En el equipo de origen, se quitan los nombres de equipo alternativo persistentes, se quita la cuenta local temporal que se ha creado y se vuelve a habilitar la Directiva de filtro de tokens de cuenta local.

9. Se quita el equipo de destino del dominio.

10. Reemplazamos las direcciones IP del equipo de destino con la información de IP proporcionada por el origen y, a continuación, cambia el nombre del equipo de destino al nombre original del equipo de origen.

11. Se unirá el equipo de destino al dominio. Cuando se combina, utiliza la cuenta de equipo Active Directory original del equipo de origen. Esto conserva la pertenencia a grupos y las ACL de seguridad. El equipo de destino ahora tiene la identidad del equipo de origen.

12. En el equipo de destino, se quitan los nombres de equipo alternativo persistentes, se quita la cuenta local temporal que se ha creado y se vuelve a habilitar la Directiva de filtro de tokens de cuenta local, se completa la transferencia.

Una vez finalizado el traslado, el equipo de destino ha realizado la identidad del equipo de origen y, a continuación, se puede retirar el equipo de origen.

## <a name="detailed-stages"></a>Fases detalladas

![Captura ](media/cutover/cutover_stage_description.png)
 __de pantalla de la descripción de la fase de copia 2: servicio de migración de almacenamiento que muestra una descripción de la fase__

Puede realizar un seguimiento del progreso de la transferencia a través de las descripciones de cada fase que aparecen tal como se muestra en la ilustración anterior. En la tabla siguiente se muestra cada una de las fases posibles junto con su progreso, Descripción y cualquier otra aclaración.

|  Progreso | Descripción                                                                                               |  Notas |
|:-----|:--------------------------------------------------------------------------------------------------------------------|:---|
|  0 % | El traslado está inactivo. |   |
| 2 %  | Conectando con el equipo de origen... |   Asegúrese de que se cumplen los [requisitos para los equipos de origen y de destino](./overview.md#security-requirements-the-storage-migration-service-proxy-service-and-firewall-ports) .|
| 5 %  | Conectando con el equipo de destino... |   |
| 6 %  | Estableciendo permisos de seguridad en el objeto de equipo en Active Directory... |   Replica los permisos de seguridad del objeto Active Directory del equipo de origen en el equipo de destino.|
| 8 %  | Comprobando que la cuenta temporal que hemos creado se ha eliminado correctamente en el equipo de origen... |   Se asegura de que se puede crear una cuenta temporal con el mismo nombre.|
| 11 % | Creando una cuenta de usuario local temporal en el equipo de origen... |   Si el equipo de origen está unido a un dominio, el nombre de usuario de la cuenta temporal es "MsftSmsStorMigratSvc". La contraseña consta de caracteres anchos de Unicode aleatorios de 127 con letras, números, símbolos y cambios de mayúsculas y minúsculas. Si el equipo de origen está en un grupo de trabajo, se usan las credenciales de origen originales.|
| 13 % | Estableciendo la Directiva de filtro de token de cuenta local en el equipo de origen... |   Deshabilita la Directiva para que podamos conectarnos al origen cuando no está unido al dominio. Obtenga más información sobre la Directiva de filtro de tokens de cuenta local [aquí](https://support.microsoft.com/help/951016/description-of-user-account-control-and-remote-restrictions-in-windows).|
| 16 % | Conectando con el equipo de origen mediante la cuenta de usuario local temporal... |   |
| 19 % | Comprobando que la cuenta temporal que hemos creado se ha eliminado correctamente en el equipo de destino... |   |
| 22 % | Creando una cuenta de usuario local temporal en el equipo de destino... | Si el equipo de destino está unido a un dominio, el nombre de usuario de la cuenta temporal es "MsftSmsStorMigratSvc". La contraseña consta de caracteres anchos de Unicode aleatorios de 127 con letras, números, símbolos y cambios de mayúsculas y minúsculas. Si el equipo de destino se encuentra en un grupo de trabajo, se usan las credenciales de destino originales. |
| 25 % | Configurando la Directiva de filtro de tokens de cuenta local en el equipo de destino... | Deshabilita la Directiva para que se pueda conectar al destino cuando no esté unido al dominio. Obtenga más información sobre la Directiva de filtro de tokens de cuenta local [aquí](https://support.microsoft.com/help/951016/description-of-user-account-control-and-remote-restrictions-in-windows).   |
| 27 % | Conectando con el equipo de destino mediante la cuenta de usuario local temporal... |   |
| 30 % | Quitando el equipo de origen del dominio... |   |
| 31 | Recopilando las direcciones IP del equipo de origen. |   Solo se aplica a los equipos de origen Linux. |
| 33 % | Reiniciando el equipo de origen... (primer reinicio) |   |
| 36% | Esperando a que el equipo de origen responda después del primer reinicio... |   Es probable que deje de responder si el equipo de origen no está incluido en una subred DHCP, pero seleccionó DHCP durante la configuración de la red.|
| 38% | Asignando interfaces de red en el equipo de origen... |   |
| 41 % | Cambiando el nombre del equipo de origen... |   |
| 42% | Reiniciando el equipo de origen... (primer reinicio) |   Solo se aplica a los equipos de origen Linux.|
| 43% | Reiniciando el equipo de origen... (segundo reinicio) |   Solo se aplica a los equipos de origen de Windows Server 2003 Unidos a un dominio.|
| 43% | Esperando a que el equipo de origen responda después del primer reinicio... |   |
| 43% | Esperando a que el equipo de origen responda después del segundo reinicio... |   |
| 44% | Agregando el equipo de origen al dominio... |   |
| 47 % | Reiniciando el equipo de origen... (primer reinicio) |   |
| 50 % | Reiniciando el equipo de origen... (segundo reinicio) |   |
| 51% | Reiniciando el equipo de origen... (tercer reinicio) |   Solo se aplica a los equipos de origen de Windows Server 2003.|
| 52% | Esperando a que el equipo de origen responda... |   |
| 52% | Esperando a que el equipo de origen responda después del primer reinicio... |   |
| 55 % | Esperando a que el equipo de origen responda después del segundo reinicio... |   |
| 56% | Esperando a que el equipo de origen responda después del tercer reinicio... |   |
| 57% | Quitando nombres de equipo alternativos en el origen... |   Garantiza que el origen sea inaccesible para otros usuarios y aplicaciones. Para obtener más información, consulte [netdom computername](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/cc835082(v=ws.11)). |
| 58 % | Quitando una cuenta local temporal creada en el equipo de origen... |   |
| 61 % | Restableciendo la Directiva de filtro de token de cuenta local en el equipo de origen... |   Habilita la Directiva.|
| 63% | Quitando el equipo de destino del dominio... |   |
| 66 % | Reiniciando el equipo de destino... (primer reinicio) |   |
| 69% | Esperando a que el equipo de destino responda después del primer reinicio... |   |
| 72% | Asignando interfaces de red en el equipo de destino... |  Asigna cada adaptador de red y dirección IP del equipo de origen al equipo de destino, reemplazando la información de red del destino.   |
| 75 % | Cambiando el nombre del equipo de destino... |   |
| 77% | Agregando el equipo de destino al dominio... |  El equipo de destino asume el objeto de Active Directory del equipo de origen anterior. Esto puede producir un error si el usuario de destino no es miembro del grupo Admins. del dominio o no tiene derechos de administrador en el equipo de origen Active Directory objeto. Puede especificar credenciales de destino alternativas en el paso "escribir credenciales" antes de que se inicie la transferencia.|
| 80 % | Reiniciando el equipo de destino... (primer reinicio) |   |
| 83 % | Reiniciando el equipo de destino... (segundo reinicio) |   |
| 84% | Esperando a que el equipo de destino responda... |   |
| 86% | Esperando a que el equipo de destino responda después del primer reinicio... |   |
| 88% | Esperando a que el equipo de destino responda después del segundo reinicio... |   |
| 91 % | Esperando a que el equipo de destino responda con el nuevo nombre... |  La replicación de DNS puede tardar mucho Active Directory tiempo. |
| 93 % | Quitando nombres de equipo alternativos en el destino... |   Garantiza que se ha reemplazado el nombre de destino.|
| 94% | Quitando una cuenta local temporal creada en el equipo de destino...|   |
| 97% | Restableciendo la Directiva de filtro de token de cuenta local en el equipo de destino... |   Habilita la Directiva.|
| (100%) | Correcto |   |

## <a name="faq"></a>Preguntas más frecuentes

### <a name="__is-domain-controller-migration-supported__"></a>__¿Se admite la migración del controlador de dominio?__

No en este momento, pero consulte la [Página de preguntas más frecuentes](./faq.md#is-domain-controller-migration-supported) para obtener una solución alternativa.


## <a name="known-issues"></a>Problemas conocidos
>Asegúrese de que ha cumplido los requisitos de la [información general del servicio de migración de almacenamiento](overview.md) e instalado la última actualización de Windows en el equipo que ejecuta el servicio de migración de almacenamiento.

Vea la [Página de problemas conocidos](./known-issues.md) para obtener más información sobre los problemas siguientes.
* [__Se produce el error "acceso denegado para la Directiva de filtro de tokens en el equipo de destino" del servicio de migración de almacenamiento__](./known-issues.md#storage-migration-service-cutover-validation-fails-with-error-access-is-denied-for-the-token-filter-policy-on-destination-computer)

* [__Error "CLUSCTL_RESOURCE_NETNAME_REPAIR_VCO error en el recurso de dirección de servidor" y se produce un error en la transferencia del clúster de Windows Server 2008 R2__](./known-issues.md#error-clusctl_resource_netname_repair_vco-failed-against-netname-resource-and-windows-server-2008-r2-cluster-cutover-fails)

* [__El total de bloqueos en "38% está asignando interfaces de red en el equipo de origen..." al usar direcciones IP estáticas__](./known-issues.md#cutover-hangs-on-38-mapping-network-interfaces-on-the-source-computer-when-using-static-ips)

* [__El total de bloqueos en "38% está asignando interfaces de red en el equipo de origen..."__](./known-issues.md#cutover-hangs-on-38-mapping-network-interfaces-on-the-source-computer)

## <a name="additional-references"></a>Referencias adicionales

- [Información general del servicio de migración de almacenamiento](overview.md)
- [Migración de un servidor de archivos mediante el servicio de migración de almacenamiento](migrate-data.md)
- [Preguntas más frecuentes (p + f) sobre Storage Migration Services](faq.md)
- [Problemas conocidos del servicio de migración de almacenamiento](known-issues.md)