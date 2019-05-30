---
title: Virtualización de forma segura los servicios de dominio de Active Directory (AD DS)
description: La reversión de USN y virtualización segura de Active Directory
ms.topic: article
ms.prod: windows-server-threshold
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 03/22/2019
ms.technology: identity-adds
ms.assetid: 7a3114c8-bda8-49bb-83a8-4e04340ab221
ms.openlocfilehash: aa84e09e8a958193fee82c7b9c03cd1dca910c55
ms.sourcegitcommit: 2977c707a299929c6ab0d1e0adab2e1c644b8306
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/24/2019
ms.locfileid: "63684182"
---
# <a name="safely-virtualizing-active-directory-domain-services-ad-ds"></a>Virtualización de forma segura los servicios de dominio de Active Directory (AD DS)

>Se aplica a: Windows Server

A partir de Windows Server 2012, AD DS proporciona mayor compatibilidad con virtualización de controladores de dominio mediante la introducción de las capacidades de virtualización segura. En este artículo se explica el rol de USN y InvocationIDs en la replicación del controlador de dominio y se examinan algunos problemas posibles que pueden producirse.

## <a name="update-sequence-number-and-invocationid"></a>Número de secuencia de actualización y el Id. de invocación

Los entornos virtuales presentan desafíos únicos para las cargas de trabajo distribuidas que dependen de un esquema de replicación basado en reloj lógico. Una replicación de AD DS, por ejemplo, utiliza un valor que aumenta de forma continua (denominado número de secuencias actualizadas o USN) y que se asigna a las transacciones en cada controlador de dominio. Instancia de base de datos de cada controlador de dominio también se proporciona una identidad, denominada InvocationID. El InvocationID, o identificador de invocación, de un controlador de dominio junto con su USN forman un identificador exclusivo que se asocia a cada transacción de escritura que se ejecuta en cada controlador de dominio y debe ser único en el bosque.

La replicación de AD DS utiliza el InvocationID y USN en cada controlador de dominio para determinar los cambios que es necesario replicar en otros controladores de dominio. Si un controlador de dominio se revierte en el tiempo sin conocimiento del controlador de dominio y se reutiliza un USN para una transacción completamente distinta, la replicación no convergerá debido a que otros controladores de dominio detectarán que ya han recibido las actualizaciones asociado al USN reutilizado en el contexto de ese InvocationID.

Por ejemplo, en la siguiente ilustración se muestra la secuencia de eventos que tiene lugar en Windows Server 2008 R2 y en sistemas operativos anteriores cuando se detecta una reversión de USN en VDC2, el controlador de dominio de destino que se ejecuta en una máquina virtual. En esta ilustración, se produce la detección de la reversión de USN en VDC2 cuando un asociado de replicación detecta que VDC2 envió un valor de actualización USN que había visto por el asociado de replicación, lo que indica que base de datos de VDC2 se revirtió en el tiempo de forma incorrecta.

![La secuencia de eventos cuando se detecta una reversión de USN](../media/Introduction-to-Active-Directory-Domain-Services--AD-DS--Virtualization--Level-100-/ADDS_Exampleofhowreplicationcanbecomeinconsistent.png)

Una máquina virtual (VM) facilita para los administradores del hipervisor revertir un dominio USN del controlador (su reloj lógico), por ejemplo, aplicar una instantánea sin conocimiento del controlador de dominio. Para obtener más información acerca de USN y la reversión de USN, incluida otra ilustración donde se muestran instancias no detectadas de la reversión de USN, consulte [USN y reversión de USN](https://technet.microsoft.com/library/virtual_active_directory_domain_controller_virtualization_hyperv(WS.10).aspx#usn_and_usn_rollback).

A partir de Windows Server 2012, los controladores de dominio virtual de AD DS hospedados en plataformas de hipervisor que expongan un identificador denominado identificador de generación de máquina virtual pueden detectar y emplear medidas de seguridad necesarias para proteger el entorno de AD DS si se revierte la máquina virtual en tiempo de la aplicación de una instantánea de máquina virtual. El diseño de VM-GenerationID utiliza un mecanismo independiente del proveedor del hipervisor para exponer este identificador en el espacio de dirección de la máquina virtual invitada, de modo que cualquier hipervisor que admita VM-GenerationID puede proporcionar una experiencia de virtualización segura. Los servicios y las aplicaciones que se ejecutan en la máquina virtual pueden muestrear este identificador para detectar si una máquina virtual se ha revertido en el tiempo.

## <a name="effects-of-usn-rollback"></a>Efectos de la reversión de USN

Cuando se producen reversiones de USN, las modificaciones en objetos y atributos no son entrantes replicado por controladores de dominio de destino que han visto anteriormente el USN.

Dado que estos controladores de dominio de destino se considera que están actualizadas, se notifica ningún error de replicación en los registros de eventos del servicio de directorio o mediante herramientas de supervisión y diagnóstico.

La reversión de USN puede afectar a la replicación de cualquier objeto o atributo de cualquier partición. El efecto secundario observado con más frecuencia es que las cuentas de usuario y cuentas de equipo que se crean en el controlador de dominio de reversión no existen en uno o varios asociados de replicación. O bien, las actualizaciones de contraseña que se originaron en el controlador de dominio de reversión no existen en asociados de replicación.

Una reversión de USN puede impedir que replicar cualquier tipo de objeto en cualquier partición de Active Directory. Estos tipos de objeto incluyen lo siguiente:

* La topología de replicación de Active Directory y programación
* La existencia de controladores de dominio en el bosque y los roles que contienen estos controladores de dominio
* La existencia de particiones de dominio y de aplicación en el bosque
* La existencia de grupos de seguridad y su pertenencia actual
* Registro del registro DNS en zonas DNS integradas en Active Directory

El tamaño de la vulnerabilidad de USN puede representar cientos, miles o incluso a decenas de miles de cambios a los usuarios, equipos, confianzas, las contraseñas y los grupos de seguridad. La vulnerabilidad de USN se define por la diferencia entre el número de USN más alto que existía cuando se realizó la copia de seguridad del estado del sistema restaurado y cambia el número del que se origina que se crearon en el controlador de dominio revertido antes de que se dejó sin conexión.

## <a name="detecting-a-usn-rollback"></a>Detectar una reversión de USN

Dado que es difícil de detectar una reversión de USN, un controlador de dominio registra el evento 2095 cuando un controlador de dominio de origen envía un número previamente confirmado de USN en un controlador de dominio de destino sin un cambio correspondiente en el identificador de invocación.

Para evitar únicos que se originan las actualizaciones a Active Directory se crean en el controlador de dominio restaurado incorrectamente, el servicio de Net Logon está en pausa. Cuando se pausa el servicio de Net Logon, cuentas de usuario y equipo no pueden cambiar la contraseña en un controlador de dominio que no se saliente replicar dichos cambios. De forma similar, las herramientas de administración de Active Directory dará prioridad a un controlador de dominio correcto cuando realizan actualizaciones a los objetos de Active Directory.

En un controlador de dominio, se registran los mensajes de eventos que es similar al siguiente si se cumplen las condiciones siguientes:

* Un controlador de dominio de origen envía un número previamente confirmado de USN en un controlador de dominio de destino.
* No hay ningún cambio correspondiente en el identificador de invocación.

Estos eventos pueden capturarse en el registro de eventos del servicio de directorio. Sin embargo, pueden sobrescribir antes de que se observan por un administrador.

Si sospecha que una reversión de USN se ha producido, pero no ve un evento correspondiente en el caso de los registros, busque la entrada DSA no se puede escribir en el registro. Esta entrada proporciona pruebas forenses que se ha producido una reversión de USN.

```
HKEY_LOCAL_MACHINE\System\CurrentControlSet\Services\NTDS\Parameters
Registry entry: Dsa Not Writable
Value: 0x4
```

> [!WARNING]
> Eliminar o cambiar manualmente el valor de entrada del registro Dsa no pueden modificarse coloca el controlador de dominio de reversión en un estado no compatible de forma permanente. Por lo tanto, no se admiten tales cambios. En concreto, la modificación del valor quita el comportamiento de cuarentena agregado por el código de detección de la reversión de USN. Las particiones de Active Directory en el controlador de dominio de reversión serán permanentemente incoherentes con asociados de replicación transitiva y directa en el mismo bosque de Active Directory.

Más información sobre este pasos clave y la resolución de registro puede encontrarse en el artículo de soporte técnico [Active Directory Replication Error 8456 o 8457: "El origen | servidor de destino está rechazando actualmente solicitudes de replicación"](https://support.microsoft.com/help/2023007/active-directory-replication-error-8456-or-8457-the-source-destination).

## <a name="virtualization-based-safeguards"></a>Medidas de seguridad en función de virtualización

Durante la instalación del controlador de dominio, AD DS almacena inicialmente el identificador VM GenerationID como parte del atributo msDS-GenerationID en el objeto de equipo del controlador de dominio en su base de datos (a menudo denominado el árbol de información de directorios o DIT). El identificador VM GenerationID es objeto de un seguimiento independiente por parte de un controlador de Windows en la máquina virtual.

Cuando un administrador restaura la máquina virtual de una instantánea anterior, el valor actual de VM GenerationID del controlador de la máquina virtual se compara con un valor en el DIT.

Si los dos valores son distintos, se restablece el invocationID y se descarta el grupo RID para evitar la reutilización del USN. Si los valores coinciden, la transacción se confirma del modo habitual.

AD DS también compara el valor actual del VM GenerationID de la máquina virtual con el valor del DIT cada vez que se reinicia el controlador de dominio y, si es distinto, restablece el invocationID, descarta el grupo RID y actualiza el DIT con este nuevo valor. También sincroniza de forma no autoritativa la carpeta SYSVOL para completar la restauración segura. Esto permite ampliar las medidas de seguridad a la aplicación de instantáneas en las máquinas virtuales que se apagaron. Estas medidas de seguridad introducidos en Windows Server 2012 permiten a los administradores de AD DS para beneficiarse de las ventajas únicas derivadas de la implementación y administración de controladores de dominio en un entorno virtualizado.

La siguiente ilustración muestra cómo se aplican las medidas de seguridad de virtualización cuando se detecta la misma reversión de USN en un controlador de dominio virtualizado que ejecuta Windows Server 2012 en un hipervisor que admita VM-GenerationID.

![Medidas de seguridad que se aplican cuando se detecta la misma reversión de USN](../media/Introduction-to-Active-Directory-Domain-Services--AD-DS--Virtualization--Level-100-/ADDS_VDC_Exampleofhowsafeguardswork.gif)

En este caso, cuando el hipervisor detecta un cambio en el valor de VM-GenerationID, se desencadenan las medidas de seguridad de virtualización, tales como el restablecimiento del InvocationID en el controlador de dominio virtualizado (de A a B en el ejemplo anterior) y la actualización del valor de VM-GenerationID guardado en la máquina virtual para que coincida con el nuevo valor (G2) almacenado por el hipervisor. Las medidas de seguridad garantizan la convergencia de la replicación en ambos controladores de dominio.

Con Windows Server 2012, AD DS emplea medidas de seguridad en controladores de dominio virtuales hospedados en hipervisores compatibles de VM-GenerationID y garantiza que la aplicación accidental de instantáneas o de otros compatibles con el hipervisor mecanismos que podrían revertir virtual estado de la máquina no deteriore el entorno de AD DS (mediante la prevención de problemas de replicación tales como una burbuja de USN u objetos persistentes).

No se recomienda restaurar un controlador de dominio mediante la aplicación de una instantánea de máquina virtual como un mecanismo alternativo para la copia de seguridad de un controlador de dominio. Se recomienda seguir usando Copias de seguridad de Windows Server u otras soluciones de copia de seguridad basadas en VSS Writer.

> [!CAUTION]
> Si un controlador de dominio en un entorno de producción se revierte accidentalmente a una instantánea, se recomienda consultar a los proveedores para las aplicaciones y servicios hospedados en esa máquina virtual, para obtener instrucciones sobre cómo comprobar el estado de estos programas después de restauración de instantáneas.

Para obtener más información, consulte [Virtualized domain controller safe restore architecture](../ad-ds/get-started/virtual-dc/Virtualized-Domain-Controller-Architecture.md#BKMK_SafeRestoreArch).

## <a name="recovering-from-a-usn-rollback"></a>Recuperarse de una reversión de USN

Existen dos enfoques para recuperarse de una reversión de USN:

* Quitar el controlador de dominio del dominio
* Restaurar el estado del sistema de una buena copia de seguridad

### <a name="remove-the-domain-controller-from-the-domain"></a>Quitar el controlador de dominio del dominio

1. Quitar Active Directory de controlador de dominio para obligarlo a ser un servidor independiente.
2. Apague el servidor degradado.
3. En un controlador de dominio correcto, limpie los metadatos del controlador de dominio degradado.
4. Si las funciones de maestro de las operaciones de hosts del controlador de dominio restaurado incorrectamente, transfiera estos roles a un controlador de dominio correcto.
5. Reinicie el servidor degradado.
6. Si es necesario para volver a instalar Active Directory en el servidor independiente.
7. Si el controlador de dominio era anteriormente un catálogo global, configure el controlador de dominio para que sea un catálogo global.
8. Si anteriormente, el controlador de dominio había hospeda roles de maestro de operaciones, transfiera las operaciones de copia de roles de maestro de en el controlador de dominio.

### <a name="restore-the-system-state-of-a-good-backup"></a>Restaurar el estado del sistema de una buena copia de seguridad

Evalúe si existen copias de seguridad de estado de sistema válido para este controlador de dominio. Si se realizó una copia de seguridad del estado del sistema válido antes de que el controlador de dominio revertido se restaurara incorrectamente, y la copia de seguridad contiene cambios recientes realizados en el controlador de dominio, restaure el estado del sistema desde la copia de seguridad más reciente.

También puede utilizar la instantánea como un origen de una copia de seguridad. O bien puede establecer la base de datos para proporcionarse a sí misma un nuevo identificador de invocación mediante el procedimiento de la sección [restaurar un controlador de dominio virtual cuando no está disponible una copia de seguridad de datos de estado de sistema adecuado](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd363553%28v%3dws.10%29#restoring-a-virtual-domain-controller-when-an-appropriate-system-state-data-backup-is-not-available)

## <a name="next-steps"></a>Pasos siguientes

* Para obtener más información acerca de la solución de problemas de controladores de dominio virtualizados, consulte el tema relativo a la [solución de problemas de controladores de dominio virtualizados](../ad-ds/manage/virtual-dc/Virtualized-Domain-Controller-Troubleshooting.md).
* [Información detallada sobre el servicio de hora de Windows (W32Time)](../../networking/windows-time-service/windows-time-service-top.md)
