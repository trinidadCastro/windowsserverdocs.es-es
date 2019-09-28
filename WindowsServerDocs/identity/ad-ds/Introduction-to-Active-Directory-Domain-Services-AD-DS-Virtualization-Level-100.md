---
title: Virtualización Active Directory Domain Services de forma segura (AD DS)
description: Reversión de USN y virtualización segura de Active Directory
ms.topic: article
ms.prod: windows-server
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 03/22/2019
ms.technology: identity-adds
ms.assetid: 7a3114c8-bda8-49bb-83a8-4e04340ab221
ms.openlocfilehash: 67e35a47467b1f5f66bfd073c6f9db06094ea3f9
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71391029"
---
# <a name="safely-virtualizing-active-directory-domain-services-ad-ds"></a>Virtualización Active Directory Domain Services de forma segura (AD DS)

>Se aplica a: Windows Server

A partir de Windows Server 2012, AD DS proporciona una mayor compatibilidad para la virtualización de controladores de dominio mediante la introducción de capacidades de virtualización segura. En este artículo se explica el rol de los USN y InvocationIDs en la replicación del controlador de dominio y se describen algunos posibles problemas que pueden producirse.

## <a name="update-sequence-number-and-invocationid"></a>Número de secuencia de actualización e invocación

Los entornos virtuales presentan desafíos únicos para las cargas de trabajo distribuidas que dependen de un esquema de replicación basado en reloj lógico. Una replicación de AD DS, por ejemplo, utiliza un valor que aumenta de forma continua (denominado número de secuencias actualizadas o USN) y que se asigna a las transacciones en cada controlador de dominio. A cada instancia de base de datos de cada controlador de dominio también se le asigna una identidad, conocida como invocación. El InvocationID, o identificador de invocación, de un controlador de dominio junto con su USN forman un identificador exclusivo que se asocia a cada transacción de escritura que se ejecuta en cada controlador de dominio y debe ser único en el bosque.

La replicación de AD DS utiliza el InvocationID y USN en cada controlador de dominio para determinar los cambios que es necesario replicar en otros controladores de dominio. Si un controlador de dominio se revierte en el tiempo fuera del reconocimiento del controlador de dominio y se reutiliza un USN para una transacción completamente distinta, la replicación no convergerá debido a que otros controladores de dominio considerarán que ya han recibido las actualizaciones. asociado al USN reutilizado en el contexto de ese invocación de este.

Por ejemplo, en la siguiente ilustración se muestra la secuencia de eventos que tiene lugar en Windows Server 2008 R2 y en sistemas operativos anteriores cuando se detecta una reversión de USN en VDC2, el controlador de dominio de destino que se ejecuta en una máquina virtual. En esta ilustración, la detección de la reversión de USN se produce en VDC2 cuando un asociado de replicación detecta que VDC2 ha enviado un valor de USN de actualización que el asociado de replicación ha detectado anteriormente, lo que indica que la base de datos Vdc2 se ha revertido en el tiempo. incorrectamente.

![La secuencia de eventos cuando se detecta una reversión de USN](../media/Introduction-to-Active-Directory-Domain-Services--AD-DS--Virtualization--Level-100-/ADDS_Exampleofhowreplicationcanbecomeinconsistent.png)

Una máquina virtual (VM) facilita a los administradores de hipervisor la reversión de los USN de un controlador de dominio (su reloj lógico) mediante, por ejemplo, la aplicación de una instantánea fuera del reconocimiento del controlador de dominio. Para obtener más información acerca de USN y la reversión de USN, incluida otra ilustración donde se muestran instancias no detectadas de la reversión de USN, consulte [USN y reversión de USN](https://technet.microsoft.com/library/virtual_active_directory_domain_controller_virtualization_hyperv(WS.10).aspx#usn_and_usn_rollback).

A partir de Windows Server 2012, AD DS los controladores de dominio virtuales hospedados en plataformas de hipervisor que exponen un identificador denominado identificador de generación de VM pueden detectar y emplear las medidas de seguridad necesarias para proteger el entorno de AD DS si la máquina virtual se ha revertido. atrás en el tiempo por la aplicación de una instantánea de la máquina virtual. El diseño de VM-GenerationID utiliza un mecanismo independiente del proveedor del hipervisor para exponer este identificador en el espacio de dirección de la máquina virtual invitada, de modo que cualquier hipervisor que admita VM-GenerationID puede proporcionar una experiencia de virtualización segura. Los servicios y las aplicaciones que se ejecutan en la máquina virtual pueden muestrear este identificador para detectar si una máquina virtual se ha revertido en el tiempo.

## <a name="effects-of-usn-rollback"></a>Efectos de la reversión de USN

Cuando se producen reversiones de USN, los controladores de dominio de destino que anteriormente han detectado el USN no replican los cambios en los objetos y atributos.

Dado que estos controladores de dominio de destino creen que están actualizados, no se produce ningún error de replicación en los registros de eventos del servicio de directorio o en las herramientas de supervisión y diagnóstico.

La reversión de USN puede afectar a la replicación de cualquier objeto o atributo en cualquier partición. El efecto secundario observado con más frecuencia es que las cuentas de usuario y las cuentas de equipo creadas en el controlador de dominio de reversión no existen en uno o varios asociados de replicación. O bien, las actualizaciones de contraseña que se originaron en el controlador de dominio de reversión no existen en los asociados de replicación.

Una reversión de USN puede impedir que se replique cualquier tipo de objeto en cualquier partición Active Directory. Estos tipos de objeto incluyen los siguientes:

* La topología de replicación de Active Directory y la programación
* La existencia de controladores de dominio en el bosque y los roles que estos controladores de dominio contienen
* Existencia de particiones de aplicación y dominio en el bosque
* La existencia de grupos de seguridad y sus pertenencias a grupos actuales
* Registro de registros DNS en zonas DNS integradas en Active Directory

El tamaño del agujero USN puede representar cientos, miles o incluso decenas de miles de cambios en los usuarios, equipos, confianzas, contraseñas y grupos de seguridad. El agujero USN se define por la diferencia entre el número USN más alto que existía cuando se realizó la copia de seguridad del estado del sistema restaurada y el número de cambios de origen que se crearon en el controlador de dominio revertido antes de que se desconectara.

## <a name="detecting-a-usn-rollback"></a>Detección de una reversión de USN

Dado que una reversión de USN es difícil de detectar, un controlador de dominio registra el evento 2095 cuando un controlador de dominio de origen envía un número USN confirmado previamente a un controlador de dominio de destino sin un cambio correspondiente en el identificador de invocación.

Para evitar que las actualizaciones únicas que se originan en Active Directory se creen en el controlador de dominio restaurado incorrectamente, el servicio de net Logon está en pausa. Cuando se pausa el servicio de inicio de sesión de red, las cuentas de usuario y de equipo no pueden cambiar la contraseña en un controlador de dominio que no sea de salida, lo que replicará dichos cambios. Del mismo modo, las herramientas de administración de Active Directory favorecerán un controlador de dominio correcto cuando realicen actualizaciones en los objetos de Active Directory.

En un controlador de dominio, se registran los mensajes de eventos similares a los siguientes si se cumplen las condiciones siguientes:

* Un controlador de dominio de origen envía un número USN confirmado previamente a un controlador de dominio de destino.
* No hay ningún cambio correspondiente en el identificador de invocación.

Estos eventos se pueden capturar en el registro de eventos del servicio de directorio. Sin embargo, se pueden sobrescribir antes de que los observe un administrador.

Si sospecha que se ha producido una reversión de USN pero no ve el evento correspondiente en los registros de eventos, busque la entrada DSA no grabable en el registro. Esta entrada proporciona pruebas forenses de que se ha producido una reversión de USN.

```
HKEY_LOCAL_MACHINE\System\CurrentControlSet\Services\NTDS\Parameters
Registry entry: Dsa Not Writable
Value: 0x4
```

> [!WARNING]
> Al eliminar o cambiar manualmente el valor de entrada del registro DSA not grabable, el controlador de dominio de reversión no se admite de forma permanente. Por lo tanto, estos cambios no se admiten. En concreto, la modificación del valor elimina el comportamiento de cuarentena agregado por el código de detección de reversión de USN. Las particiones Active Directory del controlador de dominio de reversión serán incoherentes de forma permanente con los asociados de replicación directa y transitiva en el mismo bosque Active Directory.

Puede encontrar más información sobre esta clave del registro y los pasos de resolución en el artículo de soporte [Active Directory Replication error 8456 o 8457: "El origen | el servidor de destino está rechazando actualmente las solicitudes de replicación "](https://support.microsoft.com/help/2023007/active-directory-replication-error-8456-or-8457-the-source-destination).

## <a name="virtualization-based-safeguards"></a>Medidas de seguridad basadas en la virtualización

Durante la instalación del controlador de dominio, AD DS almacena inicialmente el identificador GenerationID de la máquina virtual como parte del atributo msDS-GenerationID en el objeto de equipo del controlador de dominio en su base de datos (a menudo denominado árbol de información de directorios o DIT). El identificador VM GenerationID es objeto de un seguimiento independiente por parte de un controlador de Windows en la máquina virtual.

Cuando un administrador restaura la máquina virtual de una instantánea anterior, el valor actual de VM GenerationID del controlador de la máquina virtual se compara con un valor en el DIT.

Si los dos valores son distintos, se restablece el invocationID y se descarta el grupo RID para evitar la reutilización del USN. Si los valores coinciden, la transacción se confirma del modo habitual.

AD DS también compara el valor actual del VM GenerationID de la máquina virtual con el valor del DIT cada vez que se reinicia el controlador de dominio y, si es distinto, restablece el invocationID, descarta el grupo RID y actualiza el DIT con este nuevo valor. También sincroniza de forma no autoritativa la carpeta SYSVOL para completar la restauración segura. Esto permite ampliar las medidas de seguridad a la aplicación de instantáneas en las máquinas virtuales que se apagaron. Estas medidas de seguridad introducidas en Windows Server 2012 permiten a los administradores de AD DS beneficiarse de las ventajas únicas de la implementación y administración de controladores de dominio en un entorno virtualizado.

En la siguiente ilustración se muestra cómo se aplican las medidas de seguridad de virtualización cuando se detecta la misma reversión de USN en un controlador de dominio virtualizado que ejecuta Windows Server 2012 en un hipervisor que admite VM-GenerationID.

![Medidas de seguridad aplicadas cuando se detecta la misma reversión de USN](../media/Introduction-to-Active-Directory-Domain-Services--AD-DS--Virtualization--Level-100-/ADDS_VDC_Exampleofhowsafeguardswork.gif)

En este caso, cuando el hipervisor detecta un cambio en el valor de VM-GenerationID, se desencadenan las medidas de seguridad de virtualización, tales como el restablecimiento del InvocationID en el controlador de dominio virtualizado (de A a B en el ejemplo anterior) y la actualización del valor de VM-GenerationID guardado en la máquina virtual para que coincida con el nuevo valor (G2) almacenado por el hipervisor. Las medidas de seguridad garantizan la convergencia de la replicación en ambos controladores de dominio.

Con Windows Server 2012, AD DS emplea medidas de seguridad en los controladores de dominio virtuales hospedados en hipervisores compatibles con VM-GenerationID y garantiza que la aplicación accidental de instantáneas u otros mecanismos habilitados para hipervisor que podrían revertir una máquina virtual el estado de la máquina no interrumpe el AD DS entorno (evitando problemas de replicación, como una burbuja USN o objetos persistentes).

La restauración de un controlador de dominio mediante la aplicación de una instantánea de máquina virtual no se recomienda como mecanismo alternativo para realizar la copia de seguridad de un controlador de dominio. Se recomienda seguir usando Copias de seguridad de Windows Server u otras soluciones de copia de seguridad basadas en VSS Writer.

> [!CAUTION]
> Si un controlador de dominio de un entorno de producción se revierte accidentalmente a una instantánea, se recomienda consultar a los proveedores de las aplicaciones y servicios hospedados en esa máquina virtual para obtener instrucciones sobre cómo verificar el estado de estos programas después de restauración de instantáneas.

Para obtener más información, consulte [Virtualized domain controller safe restore architecture](../ad-ds/get-started/virtual-dc/Virtualized-Domain-Controller-Architecture.md#BKMK_SafeRestoreArch).

## <a name="recovering-from-a-usn-rollback"></a>Recuperación de una reversión de USN

Hay dos enfoques para recuperarse de una reversión de USN:

* Quitar el controlador de dominio del dominio
* Restaurar el estado del sistema de una copia de seguridad correcta

### <a name="remove-the-domain-controller-from-the-domain"></a>Quitar el controlador de dominio del dominio

1. Quite Active Directory del controlador de dominio para obligarlo a ser un servidor independiente.
2. Apague el servidor degradado.
3. En un controlador de dominio correcto, limpie los metadatos del controlador de dominio degradado.
4. Si el controlador de dominio restaurado incorrectamente hospeda roles de maestro de operaciones, transfiera estos roles a un controlador de dominio correcto.
5. Reinicie el servidor degradado.
6. Si es necesario, instale de nuevo Active Directory en el servidor independiente.
7. Si el controlador de dominio era previamente un catálogo global, configure el controlador de dominio para que sea un catálogo global.
8. Si el controlador de dominio previamente hospedaba roles de maestro de operaciones, vuelva a transferir los roles de maestro de operaciones al controlador de dominio.

### <a name="restore-the-system-state-of-a-good-backup"></a>Restaurar el estado del sistema de una copia de seguridad correcta

Evalúe si existen copias de seguridad del estado del sistema válidas para este controlador de dominio. Si se realizó una copia de seguridad válida del estado del sistema antes de que se restaurara incorrectamente el controlador de dominio revertido y la copia de seguridad contiene los cambios recientes realizados en el controlador de dominio, restaure el estado del sistema desde la copia de seguridad más reciente.

También puede usar la instantánea como origen de una copia de seguridad. O bien, puede establecer la base de datos para que se conceda a sí misma un nuevo identificador de invocación mediante el procedimiento de la sección [restauración de un controlador de dominio virtual cuando no hay disponible una copia de seguridad de datos de estado del sistema adecuada](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd363553%28v%3dws.10%29#restoring-a-virtual-domain-controller-when-an-appropriate-system-state-data-backup-is-not-available) .

## <a name="next-steps"></a>Pasos siguientes

* Para obtener más información acerca de la solución de problemas de controladores de dominio virtualizados, consulte el tema relativo a la [solución de problemas de controladores de dominio virtualizados](../ad-ds/manage/virtual-dc/Virtualized-Domain-Controller-Troubleshooting.md).
* [Información detallada sobre el servicio de hora de Windows (W32Time)](../../networking/windows-time-service/windows-time-service-top.md)
