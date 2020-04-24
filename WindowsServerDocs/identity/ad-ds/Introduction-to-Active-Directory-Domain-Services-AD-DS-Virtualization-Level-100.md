---
title: Virtualización segura de Servicios de dominio de Active Directory (AD DS)
description: Reversión de USN y virtualización segura de Active Directory
ms.topic: article
ms.prod: windows-server
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 03/22/2019
ms.technology: identity-adds
ms.assetid: 7a3114c8-bda8-49bb-83a8-4e04340ab221
ms.openlocfilehash: 25a5c2222f50b37bff2bcfe41184d6d9fa35995c
ms.sourcegitcommit: 3a3d62f938322849f81ee9ec01186b3e7ab90fe0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2020
ms.locfileid: "77465509"
---
# <a name="safely-virtualizing-active-directory-domain-services-ad-ds"></a>Virtualización segura de Servicios de dominio de Active Directory (AD DS)

>Se aplica a: Windows Server

A partir de Windows Server 2012, AD DS proporciona una mayor compatibilidad con la virtualización de controladores de dominio gracias a la introducción de recursos seguros para virtualización. En este artículo se explica el rol de los USN e id. de invocación en la replicación del controlador de dominio y se describen algunos posibles problemas que pueden producirse.

## <a name="update-sequence-number-and-invocationid"></a>Número de secuencia de actualización e id. de invocación

Los entornos virtuales presentan desafíos únicos para las cargas de trabajo distribuidas que dependen de un esquema de replicación basado en reloj lógico. Una replicación de AD DS, por ejemplo, utiliza un valor que aumenta de forma continua (denominado número de secuencias actualizadas o USN) y que se asigna a las transacciones en cada controlador de dominio. Cada instancia de base de datos de un controlador de dominio recibe, además, una identidad, denominada id. de invocación. El InvocationID, o identificador de invocación, de un controlador de dominio junto con su USN forman un identificador exclusivo que se asocia a cada transacción de escritura que se ejecuta en cada controlador de dominio y debe ser único en el bosque.

La replicación de AD DS utiliza el InvocationID y USN en cada controlador de dominio para determinar los cambios que es necesario replicar en otros controladores de dominio. Si se revierte en el tiempo un controlador de dominio sin el conocimiento del controlador de dominio y se reutiliza un USN para una transacción completamente distinta, la replicación no convergerá debido a que otros controladores de dominio detectarán que ya han recibido las actualizaciones asociadas al USN reutilizado en el contexto de ese id. de invocación.

Por ejemplo, en la siguiente ilustración se muestra la secuencia de eventos que tiene lugar en Windows Server 2008 R2 y en sistemas operativos anteriores cuando se detecta una reversión de USN en VDC2, el controlador de dominio de destino que se ejecuta en una máquina virtual. En esta ilustración, la detección de la reversión de USN se produce en VDC2 cuando un asociado de replicación detecta que VDC2 envió un valor de actualización USN que el asociado de replicación ya había visto, lo que indica que la base de datos de VDC2 se revirtió de manera incorrecta.

![La secuencia de eventos cuando se detecta una reversión de USN](../media/Introduction-to-Active-Directory-Domain-Services--AD-DS--Virtualization--Level-100-/ADDS_Exampleofhowreplicationcanbecomeinconsistent.png)

Una máquina virtual (VM) facilita a los administradores del hipervisor la reversión al USN de un controlador de dominio (su reloj lógico); esto es posible, por ejemplo, mediante la aplicación de una instantánea sin el conocimiento del controlador de dominio. Para obtener más información acerca de USN y la reversión de USN, incluida otra ilustración donde se muestran instancias no detectadas de la reversión de USN, consulte [USN y reversión de USN](https://technet.microsoft.com/library/virtual_active_directory_domain_controller_virtualization_hyperv(WS.10).aspx#usn_and_usn_rollback).

A partir de Windows Server 2012, los controladores de dominio virtuales de AD DS hospedados en plataformas del hipervisor que expongan un identificador denominado identificador de generación de VM pueden detectar y aplicar las medidas de seguridad necesarias para proteger el entorno de AD DS si se revierte en el tiempo la máquina virtual como consecuencia de la aplicación de una instantánea de VM. El diseño de VM-GenerationID utiliza un mecanismo independiente del proveedor del hipervisor para exponer este identificador en el espacio de dirección de la máquina virtual invitada, de modo que cualquier hipervisor que admita VM-GenerationID puede proporcionar una experiencia de virtualización segura. Los servicios y las aplicaciones que se ejecutan en la máquina virtual pueden muestrear este identificador para detectar si una máquina virtual se ha revertido en el tiempo.

## <a name="effects-of-usn-rollback"></a>Efectos de la reversión de USN

Cuando se producen reversiones de USN, las modificaciones en los objetos y atributos no se replican desde los controladores de dominio de destino que han detectado los USN anteriormente.

Dado que estos controladores de dominio de destino creen que están actualizados, no se genera ningún error de replicación en los registros de eventos del servicio de directorio ni desde herramientas de supervisión y diagnóstico.

La reversión de USN puede afectar a la replicación de cualquier objeto o atributo en cualquier partición. El efecto secundario observado con más frecuencia es que las cuentas de usuario y de equipo creadas en el controlador de dominio de reversión no existen en uno o varios asociados de replicación. O bien, las actualizaciones de contraseña que se originaron en el controlador de dominio de reversión no existen en los asociados de replicación.

Una reversión de USN puede impedir que se replique cualquier tipo de objeto en cualquier partición de Active Directory. Estos tipos de objeto incluyen:

* Topología y programación de replicación de Active Directory
* Existencia de controladores de dominio en el bosque y los roles que estos controladores de dominio tienen
* Existencia de particiones de aplicación y dominio en el bosque
* Existencia de grupos de seguridad y sus pertenencias a grupos actuales
* Registro de registros de DNS en zonas de DNS integradas de Active Directory

El tamaño de la vulnerabilidad de USN puede representar cientos, miles o incluso decenas de miles de cambios para los usuarios, equipos, confianzas, contraseñas y grupos de seguridad. La vulnerabilidad de USN se define por la diferencia entre el número USN más alto que existía cuando se realizó la copia de seguridad del estado del sistema restaurada y el número de cambios de origen que se crearon en el controlador de dominio revertido antes de que se desconectara.

## <a name="detecting-a-usn-rollback"></a>Detección de una reversión de USN

Dado que una reversión de USN es difícil de detectar, un controlador de dominio registra el evento 2095 cuando un controlador de dominio de origen envía un número USN confirmado previamente a un controlador de dominio de destino sin un cambio correspondiente en el identificador de invocación.

Para evitar que las actualizaciones únicas que se originan en Active Directory se creen en el controlador de dominio restaurado incorrectamente, el servicio de Net Logon se pausa. Cuando se pausa el servicio de Net Logon, las cuentas de usuario y de equipo no pueden cambiar la contraseña en un controlador de dominio que no replicará estos cambios de salida. Del mismo modo, las herramientas de administración de Active Directory favorecerán un controlador de dominio correcto cuando realicen actualizaciones en los objetos de Active Directory.

En un controlador de dominio, se registran los mensajes de eventos similares a los siguientes si se cumplen estas condiciones:

* Un controlador de dominio de origen envía un número USN confirmado previamente a un controlador de dominio de destino.
* No hay ningún cambio correspondiente en el identificador de invocación.

Estos eventos se pueden capturar en el registro de eventos del servicio de directorio. Sin embargo, se pueden sobrescribir antes de que los observe un administrador.

Si sospechas que se ha producido una reversión de USN pero no ves el evento correspondiente en los registros de eventos, busca la entrada DSA no grabable en el registro. Esta entrada proporciona pruebas forenses de que se ha producido una reversión de USN.

```
HKEY_LOCAL_MACHINE\System\CurrentControlSet\Services\NTDS\Parameters
Registry entry: Dsa Not Writable
Value: 0x4
```

> [!WARNING]
> Al eliminar o cambiar manualmente el valor de entrada del registro DSA no grabable, el controlador de dominio de reversión no se admite de forma permanente. Por lo tanto, estos cambios no se admiten. En concreto, la modificación del valor elimina el comportamiento de cuarentena agregado por el código de detección de reversión de USN. Las particiones de Active Directory del controlador de dominio de reversión serán incoherentes de forma permanente con los asociados de replicación directa y transitiva del mismo bosque de Active Directory.

Puedes encontrar más información sobre esta clave del Registro y los pasos de resolución en el artículo de soporte técnico [Error de replicación de Active Directory 8456 o 8457: "El servidor de origen | destino está rechazando actualmente las solicitudes de replicación"](https://support.microsoft.com/help/2023007/active-directory-replication-error-8456-or-8457-the-source-destination).

## <a name="virtualization-based-safeguards"></a>Medidas de seguridad basadas en la virtualización

Durante la instalación del controlador de dominio, AD DS almacena inicialmente el identificador VM GenerationID como parte del atributo msDS-GenerationID en el objeto de equipo del controlador de dominio en su base de datos (a menudo denominado árbol de información de directorios o DIT). El identificador VM GenerationID es objeto de un seguimiento independiente por parte de un controlador de Windows en la máquina virtual.

Cuando un administrador restaura la máquina virtual de una instantánea anterior, el valor actual de VM GenerationID del controlador de la máquina virtual se compara con un valor en el DIT.

Si los dos valores son distintos, se restablece el invocationID y se descarta el grupo RID para evitar la reutilización del USN. Si los valores coinciden, la transacción se confirma del modo habitual.

AD DS también compara el valor actual del VM GenerationID de la máquina virtual con el valor del DIT cada vez que se reinicia el controlador de dominio y, si es distinto, restablece el invocationID, descarta el grupo RID y actualiza el DIT con este nuevo valor. También sincroniza de forma no autoritativa la carpeta SYSVOL para completar la restauración segura. Esto permite ampliar las medidas de seguridad a la aplicación de instantáneas en las máquinas virtuales que se apagaron. Estas medidas de seguridad introducidas en Windows Server 2012 permiten a los administradores de AD DS beneficiarse de las ventajas únicas derivadas de la implementación y administración de controladores de dominio en un entorno virtualizado.

En la siguiente ilustración se muestra cómo se aplican las medidas de seguridad de virtualización cuando se detecta la misma reversión de USN en un controlador de dominio virtualizado que ejecuta Windows Server 2012 en un hipervisor que admite VM-GenerationID.

![Medidas de seguridad aplicadas cuando se detecta la misma reversión de USN](../media/Introduction-to-Active-Directory-Domain-Services--AD-DS--Virtualization--Level-100-/ADDS_VDC_Exampleofhowsafeguardswork.gif)

En este caso, cuando el hipervisor detecta un cambio en el valor de VM-GenerationID, se desencadenan las medidas de seguridad de virtualización, tales como el restablecimiento del InvocationID en el controlador de dominio virtualizado (de A a B en el ejemplo anterior) y la actualización del valor de VM-GenerationID guardado en la máquina virtual para que coincida con el nuevo valor (G2) almacenado por el hipervisor. Las medidas de seguridad garantizan la convergencia de la replicación en ambos controladores de dominio.

Con Windows Server 2012, AD DS emplea medidas de seguridad en controladores de dominio virtuales hospedados en hipervisores compatibles con VM-GenerationID y garantiza que la aplicación accidental de instantáneas o de otros mecanismos compatibles con el hipervisor que podrían revertir el estado de una máquina virtual no deteriore el entorno de AD DS (evitando problemas de replicación tales como una burbuja de USN u objetos persistentes).

Restaurar un controlador de dominio mediante la aplicación de una instantánea de máquina virtual no es un mecanismo alternativo recomendable para realizar una copia de seguridad de un controlador de dominio. Se recomienda seguir usando Copias de seguridad de Windows Server u otras soluciones de copia de seguridad basadas en VSS Writer.

> [!CAUTION]
> Si un controlador de dominio de un entorno de producción se revierte accidentalmente a una instantánea, se recomienda consultar a los proveedores de las aplicaciones y los servicios hospedados en esa máquina virtual acerca de cómo comprobar el estado de estos programas tras la restauración de la instantánea.

Para obtener más información, consulte [Virtualized domain controller safe restore architecture](../ad-ds/get-started/virtual-dc/Virtualized-Domain-Controller-Architecture.md#BKMK_SafeRestoreArch).

## <a name="recovering-from-a-usn-rollback"></a>Recuperación de una reversión de USN

Hay dos enfoques para recuperarse de una reversión de USN:

* Quitar el controlador de dominio del dominio
* Restaurar el estado del sistema a partir de una copia de seguridad correcta

### <a name="remove-the-domain-controller-from-the-domain"></a>Quitar el controlador de dominio del dominio

1. Quita Active Directory del controlador de dominio para forzar que sea un servidor independiente.
2. Apaga el servidor de nivel disminuido.
3. En un controlador de dominio correcto, limpia los metadatos del controlador de dominio degradado.
4. Si el controlador de dominio restaurado incorrectamente hospeda roles de maestro de operaciones, transfiere estos roles a un controlador de dominio correcto.
5. Reinicia el servidor de nivel disminuido.
6. Si es necesario, vuelve a instalar Active Directory en el servidor independiente.
7. Si el controlador de dominio era previamente un catálogo global, configura el controlador de dominio para que sea un catálogo global.
8. Si el controlador de dominio previamente hospedaba roles de maestro de operaciones, vuelve a transferir los roles de maestro de operaciones al controlador de dominio.

### <a name="restore-the-system-state-of-a-good-backup"></a>Restaurar el estado del sistema a partir de una copia de seguridad correcta

Evalúa si existen copias de seguridad del estado del sistema válidas para este controlador de dominio. Si se realizó una copia de seguridad válida del estado del sistema antes de que se restaurara incorrectamente el controlador de dominio revertido y la copia de seguridad contiene los cambios recientes realizados en el controlador de dominio, restaura el estado del sistema desde la copia de seguridad más reciente.

También puedes usar la instantánea como origen de una copia de seguridad. O bien, puedes definir la base de datos para que se asigne un nuevo identificador de invocación mediante el procedimiento de la sección [Restaurar un controlador de dominio virtual cuando no hay disponible una copia de seguridad de los datos del estado del sistema adecuado](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd363553%28v%3dws.10%29#restoring-a-virtual-domain-controller-when-an-appropriate-system-state-data-backup-is-not-available)

## <a name="next-steps"></a>Pasos siguientes

* Para obtener más información de solución de problemas sobre los controladores de dominio virtualizados, consulte [Virtualized Domain Controller Troubleshooting](../ad-ds/manage/virtual-dc/Virtualized-Domain-Controller-Troubleshooting.md).
* [Información detallada sobre el servicio de hora de Windows (W32Time)](../../networking/windows-time-service/windows-time-service-top.md)
