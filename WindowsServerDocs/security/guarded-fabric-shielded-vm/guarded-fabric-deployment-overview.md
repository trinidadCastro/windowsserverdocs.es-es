---
title: Inicio rápido para la implementación de tejido protegido
ms.custom: na
ms.prod: windows-server
ms.topic: article
ms.assetid: e060e052-39a0-4154-90bb-b97cc6dde68e
manager: dongill
author: justinha
ms.author: justinha
ms.technology: security-guarded-fabric
ms.date: 01/30/2019
ms.openlocfilehash: 8359532113e04e2247b4af34effc7f5b89d36f34
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71402419"
---
# <a name="quick-start-for-guarded-fabric-deployment"></a>Inicio rápido para la implementación de tejido protegido

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

En este tema se explica qué es un tejido protegido, sus requisitos y un resumen del proceso de implementación. Para obtener pasos detallados de implementación, consulte [implementación del servicio de protección de host para hosts protegidos y máquinas virtuales blindadas](https://technet.microsoft.com/windows-server-docs/security/guarded-fabric-shielded-vm/guarded-fabric-deploying-hgs-overview).

¿Prefieres el vídeo? Vea el curso de Microsoft Virtual Academy [implementación de máquinas virtuales blindadas y un tejido protegido con Windows Server 2016](https://mva.microsoft.com/en-US/training-courses/deploying-shielded-vms-and-a-guarded-fabric-with-windows-server-2016-17131?l=WFLef7vUD_4604300474).

## <a name="what-is-a-guarded-fabric"></a>Qué es un tejido protegido

Un _tejido protegido_ es un tejido de Hyper-V de Windows Server 2016 capaz de proteger las cargas de trabajo de inquilinos contra la inspección, el robo y la manipulación del malware que se ejecuta en el host, así como de los administradores del sistema. Estas cargas de trabajo virtualizadas de inquilinos (protegidas tanto en reposo como en vuelo) se denominan _máquinas virtuales blindadas_. 

## <a name="what-are-the-requirements-for-a-guarded-fabric"></a>¿Cuáles son los requisitos de un tejido protegido?

Los requisitos de un tejido protegido incluyen:

- **Un lugar para ejecutar máquinas virtuales blindadas que no son de software malintencionado.**

    Se denominan _hosts protegidos_. 
    Los hosts protegidos son hosts de Hyper-V de Windows Server 2016 Datacenter Edition que pueden ejecutar máquinas virtuales blindadas solo si pueden demostrar que se están ejecutando en un estado conocido de confianza para una entidad externa denominada servicio de protección de host (HGS). 
    HGS es un nuevo rol de servidor en Windows Server 2016 y normalmente se implementa como un clúster de tres nodos. 

- **Una manera de comprobar que un host se encuentra en un estado correcto.**

    El HGS realiza la _atestación_, donde mide el estado de los hosts protegidos.

- **Un proceso para liberar de forma segura las claves de los hosts correctos.**

    El HGS realiza la _protección de claves y la versión de clave_, donde vuelve a liberar las claves en los hosts correctos.

- **Herramientas de administración para automatizar el aprovisionamiento seguro y el hospedaje de máquinas virtuales blindadas.**

    Opcionalmente, puede agregar estas herramientas de administración a un tejido protegido:

    - System Center 2016 Virtual Machine Manager (VMM). Se recomienda VMM porque proporciona herramientas de administración adicionales más allá de lo que se obtiene al usar solo los cmdlets de PowerShell que se incluyen con Hyper-V y las cargas de trabajo de tejido protegido.
    - System Center 2016 Service Provider Foundation (SPF). Se trata de una capa de API entre Windows Azure Pack y VMM, y un requisito previo para usar Windows Azure Pack.
    - Windows Azure Pack proporciona una buena interfaz web gráfica para administrar un tejido protegido y máquinas virtuales blindadas. 

En la práctica, se debe tomar una decisión delantera: el [modo de atestación](guarded-fabric-and-shielded-vms.md#attestation-modes-in-the-guarded-fabric-solution) que usa el tejido protegido. Hay dos medios, dos modos mutuamente excluyentes, por los que HGS puede medir que un host de Hyper-V es correcto. Al inicializar HGS, debe elegir el modo:  

- La atestación de clave de host o el modo de clave es menos seguro, pero más fácil de adoptar  
- La atestación basada en TPM, o el modo TPM, es más segura, pero requiere más configuración y hardware específico.

Si es necesario, puede realizar la implementación en modo de clave mediante hosts de Hyper-V existentes que se han actualizado a Windows Server 2019 Datacenter Edition y, a continuación, convertirlos al modo TPM más seguro cuando esté disponible el hardware del servidor (incluido TPM 2,0). 

Ahora que sabe cuáles son las piezas, vamos a examinar un ejemplo del modelo de implementación.

## <a name="how-to-get-from-a-current-hyper-v-fabric-to-a-guarded-fabric"></a>Cómo obtener un tejido de Hyper-V actual en un tejido protegido

Vamos a imaginar este escenario: tiene un tejido de Hyper-V existente, como Contoso.com en la siguiente imagen, y desea crear un tejido protegido de Windows Server 2016.

![Tejido de Hyper-V existente](../media/Guarded-Fabric-Shielded-VM/guarded-fabric-existing-hyper-v.png)

## <a name="step-1-deploy-the-hyper-v-hosts-running-windows-server-2016"></a>Paso 1: implementar los hosts de Hyper-V que ejecutan Windows Server 2016 

Los hosts de Hyper-V deben ejecutar Windows Server 2016 Datacenter Edition o posterior. Si está actualizando los hosts, puede [Actualizar](https://technet.microsoft.com/windowsserver/dn527667.aspx) de la edición Standard a la edición Datacenter.

![Actualizar hosts de Hyper-V](../../security/media/Guarded-Fabric-Shielded-VM/guarded-fabric-deployment-step-one-upgrade-hyper-v.png)

## <a name="step-2-deploy-the-host-guardian-service-hgs"></a>Paso 2: implementación del servicio de protección de host (HGS)

A continuación, instale el rol de servidor HGS e impleméntelo como un clúster de tres nodos, como en el ejemplo relecloud.com de la siguiente imagen. Esto requiere tres cmdlets de PowerShell:

- Para agregar el rol HGS, use `Install-WindowsFeature` 
- Para instalar HGS, use `Install-HgsServer` 
- Para inicializar el HGS con el modo de atestación elegido, use `Initialize-HgsServer` 

Si los servidores de Hyper-V existentes no cumplen los requisitos previos para el modo TPM (por ejemplo, no tienen el TPM 2,0), puede inicializar HGS mediante la atestación basada en el administrador (modo AD), que requiere una confianza Active Directory con el dominio de tejido. 

En nuestro ejemplo, supongamos que contoso se implementa inicialmente en modo AD para cumplir de inmediato los requisitos de cumplimiento y planea realizar la conversión a la atestación basada en TPM más segura después de que se pueda comprar hardware de servidor adecuado. 

![Instalar HGS](../media/Guarded-Fabric-Shielded-VM/guarded-fabric-deployment-step-two-deploy-hgs.png)

## <a name="step-3-extract-identities-hardware-baselines-and-code-integrity-policies"></a>Paso 3: extraer las identidades, las líneas de base de hardware y las directivas de integridad de código

El proceso de extracción de identidades de hosts de Hyper-V depende del modo de atestación que se esté usando.

En el modo de AD, el identificador del host es su cuenta de equipo unido a un dominio, que debe ser miembro de un grupo de seguridad designado en el dominio del tejido.
La pertenencia al grupo designado es la única determinación de si el host está en buen estado o no. 

En este modo, el administrador del tejido es el único responsable de garantizar el estado de los hosts de Hyper-V. Dado que HGS no juega nada en la decisión de lo que se puede ejecutar, el malware y los depuradores funcionarán según lo previsto. 

Sin embargo, los depuradores que intentan conectarse directamente a un proceso (por ejemplo, WinDbg. exe) están bloqueados para las máquinas virtuales blindadas porque el proceso de trabajo de la máquina virtual (VMWP. exe) es una luz de proceso protegido (PPL).
Las técnicas de depuración alternativas, como las utilizadas por LiveKd. exe, no se bloquean. A diferencia de las máquinas virtuales blindadas, el proceso de trabajo para las máquinas virtuales compatibles con el cifrado no se ejecuta como PPL, por lo que los depuradores tradicionales como WinDbg. exe seguirán funcionando con normalidad.

Como se indicó otra manera, los pasos de validación rigurosos que se usan para el modo TPM no se usan para el modo de AD de ninguna forma.

En el modo TPM, se necesitan tres cosas: 

1.  Una _clave de aprobación pública_ (o _EKPUB_) del TPM 2,0 en cada uno de los hosts de Hyper-V. Para capturar EKpub, use `Get-PlatformIdentifier`. 
2.  _Línea de base de hardware_. Si cada uno de los hosts de Hyper-V es idéntico, se necesita una sola línea de base. Si no es así, necesitará uno para cada clase de hardware. La línea de base está en forma de un archivo de registro de Trustworthy Computing Group o TCGlog. El TCGlog contiene todo lo que el host hizo, desde el firmware UEFI, hasta el kernel, hasta el punto en el que se arranca el host por completo. Para capturar la línea de base de hardware, instale el rol de Hyper-V y la característica de compatibilidad de Hyper-V de protección de host y use `Get-HgsAttestationBaselinePolicy`. 
3.  Una _Directiva de integridad de código_. Si cada uno de los hosts de Hyper-V es idéntico, solo necesitará una directiva de CI. Si no es así, necesitará uno para cada clase de hardware. Tanto Windows Server 2016 como Windows 10 tienen una nueva forma de cumplimiento de directivas de CI, llamada _integridad de código forzada por hipervisor (HVCI)_ . HVCI proporciona un cumplimiento fuerte y garantiza que un host solo puede ejecutar archivos binarios que un administrador de confianza le permita ejecutar. Estas instrucciones se incluyen en una directiva de CI que se agrega a HGS. HGS mide la Directiva de CI de cada host antes de que se le permita ejecutar máquinas virtuales blindadas. Para capturar una directiva de CI, use `New-CIPolicy`. La Directiva se debe convertir a su formato binario mediante `ConvertFrom-CIPolicy`.

![Extraer identidades, línea base y Directiva de CI](../media/Guarded-Fabric-Shielded-VM/guarded-fabric-deployment-step-three-extract-identity-baseline-ci-policy.png)

Eso es todo: el tejido protegido se crea, en lo que se refiere a la infraestructura para ejecutarlo.  
Ahora puede crear un disco de plantilla de máquina virtual blindada y un archivo de datos de blindaje para que las máquinas virtuales blindadas se puedan aprovisionar de forma sencilla y segura. 

## <a name="step-4-create-a-template-for-shielded-vms"></a>Paso 4: creación de una plantilla para máquinas virtuales blindadas

Una plantilla de máquina virtual blindada protege los discos de plantilla mediante la creación de una firma del disco en un momento conocido de confianza. 
Si el disco de plantilla está infectado más adelante por malware, su firma tendrá una plantilla original que será detectada por el proceso de aprovisionamiento de máquinas virtuales blindadas seguro. 
Los discos de plantilla blindada se crean mediante la ejecución del **Asistente para creación de discos de plantilla blindada** o la `Protect-TemplateDisk` en un disco de plantilla normal. 

Cada una de ellas se incluye con la característica **herramientas de máquinas virtuales blindadas** en el [herramientas de administración remota del servidor para Windows 10](https://www.microsoft.com/download/details.aspx?id=45520).
Después de descargar RSAT, ejecute este comando para instalar la característica **herramientas de máquinas virtuales blindadas** :

```powershell
Install-WindowsFeature RSAT-Shielded-VM-Tools -Restart
```

Un administrador de confianza, como el administrador del tejido o el propietario de la máquina virtual, necesitará un certificado (a menudo proporcionado por un proveedor de servicios de hospedaje) para firmar el disco de plantilla VHDX. 

![Asistente para disco de plantilla blindada](../media/Guarded-Fabric-Shielded-VM/guarded-fabric-shielded-template-wizard.png)

La firma del disco se calcula a través de la partición del sistema operativo del disco virtual.
Si cambia algo en la partición del sistema operativo, la firma también cambiará.
Esto permite a los usuarios identificar de forma segura los discos en los que confían especificando la firma adecuada.

Revise los [requisitos del disco de plantilla](guarded-fabric-create-a-shielded-vm-template.md) antes de empezar. 

## <a name="step-5-create-a-shielding-data-file"></a>Paso 5: creación de un archivo de datos de blindaje 

Un archivo de datos de blindaje, también conocido como archivo. PDK, captura información confidencial sobre la máquina virtual, como la contraseña de administrador. 

![Datos de blindaje](../media/Guarded-Fabric-Shielded-VM/guarded-fabric-deployment-step-five-create-shielding-data.png)

El archivo de datos de blindaje también incluye la configuración de la Directiva de seguridad para la máquina virtual blindada. Debe elegir una de las dos directivas de seguridad al crear un archivo de datos de blindaje:

- Blindadas
   
    La opción más segura, que elimina muchos vectores de ataque administrativos.

- Cifrado admitido

    Un menor nivel de protección que todavía proporciona las ventajas de cumplimiento de poder cifrar una máquina virtual, pero permite a los administradores de Hyper-V hacer cosas como usar la conexión de la consola de VM y PowerShell Direct. 

    ![Nueva máquina virtual compatible con cifrado](../media/Guarded-Fabric-Shielded-VM/guarded-fabric-new-shielded-vm.png)

Puede agregar piezas de administración opcionales como VMM o Windows Azure Pack. Si desea crear una máquina virtual sin instalar esas piezas, consulte [paso a paso: creación de máquinas virtuales blindadas sin VMM](https://blogs.technet.microsoft.com/datacentersecurity/2016/06/06/step-by-step-creating-shielded-vms-without-vmm/).

## <a name="step-6-create-a-shielded-vm"></a>Paso 6: creación de una máquina virtual blindada

La creación de máquinas virtuales blindadas difiere muy poco de las máquinas virtuales normales. En Windows Azure Pack, la experiencia es incluso más fácil que la creación de una máquina virtual normal porque solo es necesario proporcionar un nombre, un archivo de datos de blindaje (que contiene el resto de la información de especialización) y la red de VM. 

![Nueva máquina virtual blindada en Windows Azure Pack](../media/Guarded-Fabric-Shielded-VM/guarded-fabric-new-vm-config.png)

## <a name="next-step"></a>Paso siguiente

> [!div class="nextstepaction"]
> [Requisitos previos de HGS](guarded-fabric-prepare-for-hgs.md)
