---
title: Información general sobre máquinas virtuales blindadas y tejido protegido
ms.custom: na
ms.prod: windows-server
ms.topic: article
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: ace6eb30ae6df2dc29aacc05eb7852e03145df4f
ms.sourcegitcommit: 06ae7c34c648538e15c4d9fe330668e7df32fbba
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/05/2020
ms.locfileid: "78370659"
---
# <a name="guarded-fabric-and-shielded-vms-overview"></a>Información general sobre máquinas virtuales blindadas y tejido protegido

>Se aplica a: Windows Server 2019, Windows Server (canal semianual), Windows Server 2016

## <a name="overview-of-the-guarded-fabric"></a>Información general sobre el tejido protegido

La seguridad de virtualización es un área de inversión importante en Hyper-V. Además de proteger los hosts u otras máquinas virtuales en una máquina virtual que ejecuta software malintencionado, también se necesitan proteger las máquinas virtuales de un host en peligro. Este es un peligro fundamental para cada plataforma de virtualización en la actualidad, ya sea Hyper-V, VMware o cualquier otro. Dicho simplemente, si una máquina virtual sale de una organización (de forma intencionada o accidental), esa máquina virtual puede ejecutarse en cualquier otro sistema. La protección de los recursos de gran valor de su organización, como controladores de dominio, servidores de archivos confidenciales y sistemas de recursos humanos, es la máxima prioridad.

Para ayudar a proteger contra el tejido de virtualización en peligro, Windows Server 2016 Hyper-V presentó máquinas virtuales blindadas. Una máquina virtual blindada es una máquina virtual de segunda generación (compatible con Windows Server 2012 y versiones posteriores) que tiene un TPM virtual, se cifra mediante BitLocker y solo puede ejecutarse en hosts correctos y aprobados en el tejido. Las máquinas virtuales blindadas y el tejido protegido permiten a los administradores de empresa de nube privada y a los proveedores de servicio en la nube proporcionar un entorno más seguro para las máquinas virtuales inquilinas. 

Un tejido protegido consta de:

- Un Servicio de protección de host (HGS) (normalmente, un clúster de tres nodos)
- Uno o más hosts protegidos
- Un conjunto de máquinas virtuales blindadas. El diagrama siguiente muestra cómo el Servicio de protección de host usa la atestación para asegurarse de que los únicos hosts conocidos y válidos pueden iniciar máquinas virtuales blindadas y la protección de claves para liberar de forma segura las claves para las máquinas virtuales blindadas.

Cuando un inquilino crea máquinas virtuales blindadas que se ejecutan en un tejido protegido, los hosts de Hyper-V y las mismas máquinas virtuales blindadas están protegidas por el HGS. El HGS proporciona dos servicios distintos: atestación y protección de claves. El servicio de atestación garantiza que solo los hosts de Hyper-V de confianza pueden ejecutar máquinas virtuales blindadas, mientras que el servicio de protección de claves proporciona las claves necesarias para encenderlas y migrarlas en vivo a otros hosts protegidos.

![Tejido de host protegido](../media/Guarded-Fabric-Shielded-VM/Guarded-Host-Overview-Diagram.png)

## <a name="video-introduction-to-shielded-virtual-machines"></a>Vídeo: Introducción a las máquinas virtuales blindadas 

<iframe src="https://channel9.msdn.com/Shows/Mechanics/Introduction-to-Shielded-Virtual-Machines-in-Windows-Server-2016/player" width="650" height="440" allowFullScreen frameBorder="0"></iframe>

## <a name="attestation-modes-in-the-guarded-fabric-solution"></a>Modos de atestación de la solución de tejido protegido

HGS admite diferentes modos de atestación para un tejido protegido:

- Atestación de confianza de TPM (basada en hardware)
- Atestación de clave de host (basada en pares de claves asimétricas)

Se recomienda la atestación de confianza de TPM porque ofrece comprobaciones más fuertes, tal como se explica en la tabla siguiente, pero requiere que los hosts de Hyper-V tengan TPM 2.0. Si actualmente no tiene TPM 2,0 o cualquier TPM, puede usar la atestación de clave de host. Si decide cambiar a la atestación de confianza de TPM al adquirir nuevo hardware, puede cambiar el modo de atestación en el Servicio de protección de host con poca o ninguna interrupción del tejido.

| **Modo de atestación que elija para los hosts**                                            | **Garantías de host** |
|-------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|**Atestación de confianza de TPM:** ofrece los mecanismos de protección más eficaces posible, pero también requiere más pasos de configuración. El hardware y el firmware del host deben incluir TPM 2,0 y UEFI 2.3.1 con el arranque seguro habilitado. | Los hosts protegidos se aprueban en función de su identidad de TPM, la secuencia de arranque medida y las directivas de integridad de código para asegurarse de que solo ejecutan código aprobado.| 
| **Atestación de clave de host:** Diseñado para admitir el hardware del host existente donde TPM 2,0 no está disponible. Requiere menos pasos de configuración y es compatible con el hardware de servidor más habitual. | Los hosts protegidos se aprueban en función de la posesión de la clave. | 

Otro modo denominado **atestación de confianza de administrador** está en desuso a partir de Windows Server 2019. Este modo se basó en la pertenencia a un host protegido en un grupo de seguridad de Active Directory Domain Services (AD DS) designado. La atestación de clave de host proporciona una identificación de host similar y es más fácil de configurar. 

## <a name="assurances-provided-by-the-host-guardian-service"></a>Comprobaciones proporcionadas por el Servicio de protección de host

HGS, junto con los métodos para crear las máquinas virtuales blindadas, ayudan a proporcionar las siguientes comprobaciones.

| **Tipo de garantía para máquinas virtuales**                         | **Garantías de máquinas virtuales blindadas, desde el servicio de protección de claves y los métodos de creación de máquinas virtuales blindadas** |
|----------------------------|--------------------------------------------------|
| **Discos cifrados de BitLocker (discos de sistema operativo y discos de datos)**   | Las máquinas virtuales blindadas usan BitLocker para proteger sus discos. Las claves de BitLocker necesarias para arrancar la máquina virtual y descifrar los discos están protegidas por el TPM virtual de la máquina virtual blindada mediante tecnologías probadas en el sector, como el arranque medido seguro. Aunque solo las máquinas virtuales blindadas cifran y protegen automáticamente el disco de sistema operativo, también puede [cifrar unidades de datos](https://technet.microsoft.com/itpro/windows/keep-secure/bitlocker-overview) conectadas a la máquina virtual blindada. |
| **Implementación de nuevas máquinas virtuales blindadas desde discos o imágenes de la plantilla "de confianza"** | Al implementar nuevas máquinas virtuales blindadas, los inquilinos pueden especificar en qué discos de plantilla confían. Los discos de plantilla blindados tienen firmas que se calculan en un momento dado cuando su contenido se considera de confianza. Las firmas de disco se almacenan después en un catálogo de firmas, que proporcionan con seguridad los inquilinos al tejido al crear máquinas virtuales blindadas. Durante el aprovisionamiento de las máquinas virtuales blindadas, la firma del disco se vuelve a calcular y se compara con las firmas de confianza en el catálogo. Si las firmas coinciden, se implementa la máquina virtual blindada. Si las firmas no coinciden, se considera que el disco de la plantilla blindada no es de confianza y se produce un error en la implementación. |
| **Protección de contraseñas y otros secretos cuando se crea una máquina virtual blindada** | Al crear máquinas virtuales, es necesario asegurarse de que los secretos de la máquina virtual, como las firmas de disco de confianza, los certificados RDP y la contraseña de la cuenta de administrador local de la máquina virtual, no se revelan al tejido. Estos secretos se almacenan en un archivo cifrado que se denomina archivo de datos de blindaje (un archivo.PDK), que está protegido por las claves de inquilino y cargado al tejido por el inquilino. Cuando se crea una máquina virtual blindada, el inquilino selecciona los datos de blindaje que quiere utilizar que proporcionan con seguridad estos secretos solo a los componentes de confianza en el tejido protegido. |
| **Control de inquilino en el que se puede iniciar la máquina virtual** | Los datos de blindaje también contienen una lista de los tejidos protegidos en los que se permite ejecutar una máquina virtual blindada determinada. Esto es útil, por ejemplo, en los casos en que una máquina virtual blindada normalmente reside en una nube privada local pero que puede migrarse a otra nube (pública o privada) con fines de recuperación ante desastres. La nube o el tejido de destino debe admitir máquinas virtuales blindadas y la máquina virtual blindada debe permitir que ese tejido la ejecute. |

## <a name="what-is-shielding-data-and-why-is-it-necessary"></a>¿Qué son los datos de blindaje y por qué son necesarios?

Un archivo de datos de blindaje (también denominado archivo de datos de aprovisionamiento o archivo PDK) es un archivo cifrado que crea un inquilino o el propietario de la máquina virtual para proteger la información de configuración de máquina virtual importante, como la contraseña del administrador, RDP y otros certificados relacionados con la identidad o las credenciales de unión a un dominio, entre otra. El administrador de tejido utiliza el archivo de datos de blindaje al crear una máquina virtual blindada, pero no puede ver o utilizar la información contenida en el archivo.

Entre otros, los archivos de datos de blindaje contienen secretos, como:

- Credenciales de administrador
- Un archivo de respuesta (unattend.xml)
- Una directiva de seguridad que determina si las máquinas virtuales creadas con estos datos de blindaje están configurados como blindados o compatibles con el cifrado
    - Recuerde que las máquinas virtuales configuradas como blindadas están protegidas frente a los administradores de tejido, mientras que las máquinas virtuales se admiten el cifrado no lo están.
- Un certificado RDP para proteger la comunicación de Escritorio remoto con la máquina virtual
- Un catálogo de firmas de volumen que contiene una lista de las firmas de disco de plantilla de confianza firmado desde el que puede crearse una nueva máquina virtual
- Un protector de clave (o KP) que define qué tejidos protegidos están autorizados a ejecutarse en una máquina virtual blindada

El archivo de datos de blindaje (archivo PDK) proporciona comprobaciones en relación con la máquina virtual que se creará en la forma como se ideó el inquilino. Por ejemplo, cuando el inquilino coloca un archivo de respuesta (unattend.xml) en el archivo de datos de blindaje y lo ofrece al proveedor de hospedaje, este no podrá ver o realizar cambios en ese archivo de respuesta. De forma similar, el proveedor de hospedaje no puede sustituir un VHDX diferente al crear la máquina virtual blindada, porque el archivo de datos de blindaje contiene las firmas de los discos de confianza de los que se pueden crear las máquinas virtuales blindadas.

La siguiente ilustración muestra el archivo de datos de blindaje y los elementos de configuración relacionados.

![Archivo de datos de blindaje](../media/Guarded-Fabric-Shielded-VM/shielded-vms-shielding-data-file.png)

## <a name="what-are-the-types-of-virtual-machines-that-a-guarded-fabric-can-run"></a>¿Cuáles son los tipos de máquinas virtuales que puede ejecutar un tejido protegido?

Los tejidos protegidos son capaces de ejecutar máquinas virtuales en una de tres maneras posibles:

1.  Una máquina virtual normal que no ofrece ninguna protección más allá de las versiones anteriores de Hyper-V
2.  Una máquina virtual que admite el cifrado cuyas protecciones se pueden configurar un administrador de tejido
3.  Una máquina virtual blindada cuyas protecciones se han cambiado y no puede deshabilitarse por un administrador de tejido

Las máquinas virtuales que admiten el cifrado están destinadas para usarse donde los administradores de tejido son de plena confianza.  Por ejemplo, una empresa puede implementar un tejido protegido para asegurar que los discos de máquinas virtuales están cifrados en reposo con fines de cumplimiento. Los administradores de tejido pueden seguir utilizando prácticas características de administración, como conexiones de consola de máquina virtual, PowerShell Direct y otras herramientas de solución de problemas y administración diaria.

Las máquinas virtuales blindadas están diseñadas para su uso en tejidos en donde se deben proteger los datos y el estado de la máquina virtual tanto de los administradores de tejido como del software que no es de confianza que podría estar ejecutándose en los hosts de Hyper-V. Por ejemplo, las máquinas virtuales blindadas nunca permitirán una conexión de la consola de máquina virtual, mientras que un administrador de tejido puede activar o desactivar la protección para máquinas virtuales que admiten el cifrado.

En la tabla siguiente se resumen las diferencias entre las máquinas virtuales blindadas y compatibles con el cifrado.

| Capability        | Generación 2 - Cifrado admitido     | Generación 2 - Blindado         |
|----------|--------------------|----------------|
|Arranque seguro        | Sí, necesario pero configurable        | Sí, necesario y exigido    |
|Vtpm               | Sí, necesario pero configurable        | Sí, necesario y exigido    |
|Cifrar el estado de la máquina virtual y tráfico de migración en vivo | Sí, necesario pero configurable |  Sí, necesario y exigido  |
|Componentes de integración | Configurable por el administrador de tejido      | Algunos componentes de integración bloqueados (por ejemplo, intercambio de datos, PowerShell Direct) |
|Conexión a máquina virtual (consola), dispositivos HID (por ejemplo, teclado, mouse) | Activado, no se puede deshabilitar | Habilitada en hosts a partir de la versión 1803 de Windows Server; Deshabilitado en hosts anteriores |
|Puertos COM/serie   | Compatible                             | Deshabilitado (no se puede habilitar) |
|Asociar un depurador (al proceso de la máquina virtual)<sup>1</sup>| Compatible          | Deshabilitado (no se puede habilitar) |

<sup>1</sup> los depuradores tradicionales que se asocian directamente a un proceso, como WinDbg. exe, se bloquean para las máquinas virtuales blindadas porque el proceso de trabajo de la máquina virtual (VMWP. exe) es una luz de proceso protegido (PPL). Las técnicas de depuración alternativas, como las utilizadas por LiveKd. exe, no se bloquean. A diferencia de las máquinas virtuales blindadas, el proceso de trabajo para las máquinas virtuales compatibles con el cifrado no se ejecuta como PPL, por lo que los depuradores tradicionales como WinDbg. exe seguirán funcionando con normalidad. 

Tanto las máquinas virtuales blindadas como las máquinas virtuales que admiten el cifrado seguirán admitiendo las funcionalidades de administración comunes del tejido, tales como la migración en vivo, réplica de Hyper-V o los puntos de control de máquina virtual, entre otras.

## <a name="the-host-guardian-service-in-action-how-a-shielded-vm-is-powered-on"></a>El Servicio de protección de host en acción: cómo se enciende una máquina virtual blindada

![Archivo de datos de blindaje](../media/Guarded-Fabric-Shielded-VM/shielded-vms-how-a-shielded-vm-is-powered-on.png)

1. VM01 está encendida.

    Antes de que un host protegido pueda encender una máquina virtual blindada, primero debe atestarse afirmativamente de que es correcto. Para demostrar que su estado es correcto, debe presentar un certificado de mantenimiento para el servicio de protección de claves (KPS). El certificado de mantenimiento se obtiene a través del proceso de atestación.

2. El host solicita atestación.

    El host protegido solicita atestación. El modo de atestación viene determinado por el Servicio de protección de host:

    **Atestación de confianza de TPM**: el host de Hyper-V envía información que incluye:

       - Información de identificación de TPM (su clave de aprobación)
       - Información sobre los procesos que se iniciaron durante la secuencia de arranque más reciente (el registro TCG)
       - Información sobre la Directiva de integridad de código (CI) que se aplicó en el host. 

       Attestation happens when the host starts and every 8 hours thereafter. If for some reason a host doesn't have an attestation certificate when a VM tries to start, this also triggers attestation.

    **Atestación de clave de host**: el host de Hyper-V envía la mitad pública del par de claves. HGS valida que la clave de host está registrada. 
    
    **Atestación de administrador de confianza**: el host de Hyper-V envía un vale de Kerberos, que identifica los grupos de seguridad en los que se encuentra el host. HGS valida que el host pertenezca a un grupo de seguridad que se configuró anteriormente en el administrador de HGS de confianza.

3. La atestación se realiza correctamente (o se produce un error).

    El modo de atestación determina qué comprobaciones son necesarias para atestiguar correctamente que el host es correcto. Con la atestación de confianza de TPM, se validan la identidad de TPM, las medidas de arranque y la Directiva de integridad de código del host. Con la atestación de clave de host, solo se valida el registro de la clave de host. 

4. Certificado de atestación enviado al host.

    Suponiendo que la atestación se realizó correctamente, se envía un certificado de mantenimiento al host y el host se considera "protegido" (autorizado para ejecutar máquinas virtuales blindadas). El host usa el certificado de mantenimiento para autorizar el servicio de protección de claves para liberar de forma segura las claves necesarias para trabajar con máquinas virtuales blindadas.

5. El host solicita la clave de máquina virtual.

    El host protegido no tiene las claves necesarias para encender una máquina virtual blindada (VM01 en este caso). Para obtener las claves necesarias, el host protegido debe proporcionar lo siguiente a KPS:

    - El certificado de mantenimiento actual
    - Un secreto cifrado (un protector de clave o KP) que contiene las claves necesarias para encender VM01. El secreto se cifra utilizando otras claves que solo conoce KPS.

6. Liberación de la clave.

    KPS examina el certificado de mantenimiento para determinar su validez. El certificado no debe haber caducado y KPS deben confiar en el servicio de atestación que lo emitió.

7. La clave se devuelve al host.

    Si el certificado de mantenimiento es válido, KPS intenta descifrar el secreto y devolver de forma segura las claves necesarias para encender la máquina virtual. Tenga en cuenta que las claves se cifran en el VBS del host protegido.

8. El host enciende VM01.

## <a name="guarded-fabric-and-shielded-vm-glossary"></a>Glosario de máquinas virtuales blindadas y tejido protegido

| Término              | Definición           |
|----------|------------|
| Servicio de protección de host (HGS) | Un rol de Windows Server que está instalado en un clúster protegido de servidores sin sistema operativo que es capaz de medir el estado de un host de Hyper-V y liberar las claves de los hosts de Hyper-V correctos durante el encendido o la migración en vivo de las máquinas virtuales blindadas. Estas dos funcionalidades son fundamentales para una solución de máquina virtual blindada y se conocen como **servicio de atestación** y **servicio de protección de claves** respectivamente. |
| host protegido | Un host de Hyper-V en el que se pueden ejecutar máquinas virtuales blindadas. Un host solo se puede considerar _protegido_ cuando el servicio de atestación lo consideró correcto. Las máquinas virtuales blindadas no se pueden encender ni migrar en vivo a un host de Hyper-V que todavía no se atestado o que no pudo realizar la atestación. |
| tejido protegido    | Este es el término colectivo que se utiliza para describir un tejido de hosts de Hyper-V y su Servicio de protección de host que tiene la capacidad de administrar y ejecutar máquinas virtuales blindadas. |
| máquina virtual blindada | Una máquina virtual que solo puede ejecutarse en hosts protegidos y está protegida contra la inspección, manipulación y el robo de administradores de tejido malintencionado y malware de host. |
| administrador de tejido | Un administrador de nube pública o privada que puede administrar máquinas virtuales. En el contexto de un tejido protegido, un administrador de tejido no tiene acceso a máquinas virtuales blindadas o las directivas que determinan en qué hosts pueden ejecutarse las máquinas virtuales. |
| administrador de HGS | Un administrador de confianza en la nube pública o privada que tiene la autoridad de administrar las directivas y el material criptográfico para los hosts protegidos, es decir, hosts en los que puede ejecutarse una máquina virtual blindada.|
| archivo de datos de aprovisionamiento o archivo de datos de blindaje (archivo PDK) | Un archivo cifrado que un usuario o inquilino crea para contener información importante de configuración de máquina virtual y para proteger esa información del acceso de otros usuarios. Por ejemplo, un archivo de datos de blindaje puede contener la contraseña que se asignará a la cuenta de administrador local cuando se crea la máquina virtual. |
| Seguridad basada en virtualización (VBS) | Un entorno de procesamiento y almacenamiento basado en Hyper-V que está protegido de los administradores. El modo seguro virtual proporciona el sistema con la capacidad de almacenar las claves de sistema operativo que no son visibles para un administrador del sistema operativo.|
| TPM virtual | Una versión virtualizada de un Módulo de plataforma segura (TPM). A partir de Hyper-V en Windows Server 2016, puede proporcionar un dispositivo TPM 2,0 virtual para que las máquinas virtuales se puedan cifrar, del mismo modo que un TPM físico permite cifrar una máquina física.|

## <a name="see-also"></a>Vea también

- [VM blindadas y tejido protegido](guarded-fabric-and-shielded-vms-top-node.md)
- Blog: [blog de seguridad de centro de seguridad y nube privada](https://blogs.technet.microsoft.com/datacentersecurity/)
- Vídeo: [Introducción a los virtual machines blindados](https://channel9.msdn.com/Shows/Mechanics/Introduction-to-Shielded-Virtual-Machines-in-Windows-Server-2016)
- Vídeo: [profundizar en máquinas virtuales blindadas con Windows Server 2016 Hyper-V](https://channel9.msdn.com/events/Ignite/2016/BRK3124)
