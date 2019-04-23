---
title: Inicio rápido para la implementación de tejido protegido
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
ms.assetid: e060e052-39a0-4154-90bb-b97cc6dde68e
manager: dongill
author: justinha
ms.author: justinha
ms.technology: security-guarded-fabric
ms.date: 01/30/2019
ms.openlocfilehash: d63726e7046896c9ef7aa0c3b3d85237bc3f788d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59879066"
---
# <a name="quick-start-for-guarded-fabric-deployment"></a>Inicio rápido para la implementación de tejido protegido

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

En este tema se explica qué es un tejido protegido, sus requisitos y un resumen del proceso de implementación. Para obtener pasos detallados de implementación, consulte [implementar el servicio de protección de Host para hosts protegidos y máquinas virtuales blindadas](https://technet.microsoft.com/windows-server-docs/security/guarded-fabric-shielded-vm/guarded-fabric-deploying-hgs-overview).

¿Prefieres el vídeo? Vea el curso de Microsoft Virtual Academy [implementar máquinas virtuales blindadas y un tejido protegido con Windows Server 2016](https://mva.microsoft.com/en-US/training-courses/deploying-shielded-vms-and-a-guarded-fabric-with-windows-server-2016-17131?l=WFLef7vUD_4604300474).

## <a name="what-is-a-guarded-fabric"></a>¿Qué es un tejido protegido

Un _protegido fabric_ es un tejido de Windows Server 2016 Hyper-V capaz de proteger las cargas de trabajo de inquilinos contra inspección, el robo y la alteración de malware que se ejecuta en el host, así como los administradores de sistemas. Estos virtualizar las cargas de trabajo de inquilino: protegida tanto en reposo y en tránsito, se denominan _máquinas virtuales blindadas_. 

## <a name="what-are-the-requirements-for-a-guarded-fabric"></a>¿Cuáles son los requisitos para un tejido protegido

Los requisitos para un tejido protegido incluyen:

- **Un lugar para ejecutar máquinas virtuales que está libre de software malintencionado blindadas.**

    Se denominan _hosts protegidos_. 
    Los hosts protegidos son hosts Hyper-V de Windows Server 2016 Datacenter edition que se pueden ejecutar las máquinas virtuales blindadas sólo si puede demostrar que se ejecutan en un estado conocido de confianza a una autoridad externa llamada el servicio de protección de Host (HGS). 
    El HGS es un nuevo rol de servidor en Windows Server 2016 y normalmente se implementa como un clúster de tres nodos. 

- **Una manera de comprobar un host está en un estado correcto.**

    Realiza el HGS _atestación_, donde mide el estado de los hosts protegidos.

- **Un proceso para liberar de forma segura las claves a los hosts en buen estado.**

    Realiza el HGS _clave de protección y soltar tecla_, donde suelta las teclas a hosts en buen estado.

- **Las herramientas de administración para automatizar el aprovisionamiento seguro y el alojamiento de las máquinas virtuales blindadas.**

    Si lo desea, puede agregar estas herramientas de administración a un tejido protegido:

    - System Center 2016 Virtual Machine Manager (VMM). VMM se recomienda porque proporciona herramientas más allá de lo que obtendrá de usar solo los cmdlets de PowerShell que vienen con Hyper-V y las cargas de trabajo tejido protegido de administración adicional).
    - System Center 2016 Service Provider Foundation (SPF). Se trata de un nivel de API entre Windows Azure Pack y VMM y un requisito previo para usar Windows Azure Pack.
    - Windows Azure Pack proporciona una interfaz web gráfica buena para administrar a un tejido protegido y máquinas virtuales blindadas. 

En la práctica, debe realizarse una decisión anticipado: el [modo de atestación](guarded-fabric-and-shielded-vms.md#attestation-modes-in-the-guarded-fabric-solution) utilizado por el tejido protegido. Existen dos formas: dos modos mutuamente excluyentes, por lo que puede medir HGS que un host de Hyper-V está en buen estado. Cuando se inicializa HGS, deberá elegir el modo:  

- Es menos seguro pero más fácil de adoptar atestación de clave de host o el modo de clave  
- Atestación de TPM, o en modo TPM, es más seguro, pero requiere más configuración y el hardware específico

Si es necesario, puede implementar en modo de clave con los hosts de Hyper-V existentes que se han actualizado a Windows Server 2019 Datacenter edition y, a continuación, convertir en el modo más seguro de TPM cuando está disponible el soporte del hardware de servidor (incluido el TPM 2.0). 

Ahora que sabe cuáles son las piezas, veamos un ejemplo del modelo de implementación.

## <a name="how-to-get-from-a-current-hyper-v-fabric-to-a-guarded-fabric"></a>Obtención de un tejido de Hyper-V actual a un tejido protegido

Imaginemos que este escenario, tiene un tejido de Hyper-V existente, como "contoso.com" en la siguiente imagen, y desea crear un tejido protegido de Windows Server 2016.

![Tejido de Hyper-V existente](../media/Guarded-Fabric-Shielded-VM/guarded-fabric-existing-hyper-v.png)

## <a name="step-1-deploy-the-hyper-v-hosts-running-windows-server-2016"></a>Paso 1: Implementar los hosts de Hyper-V que ejecuta Windows Server 2016 

Los hosts de Hyper-V deben ejecutar Windows Server 2016 Datacenter edition o posterior. Si va a actualizar los hosts, puede [actualizar](https://technet.microsoft.com/windowsserver/dn527667.aspx) desde Standard edition a Datacenter edition.

![Actualizar los hosts de Hyper-V](../../security/media/Guarded-Fabric-Shielded-VM/guarded-fabric-deployment-step-one-upgrade-hyper-v.png)

## <a name="step-2-deploy-the-host-guardian-service-hgs"></a>Paso 2: Implementar el servicio de protección de Host (HGS)

A continuación, instalar el rol de servidor HGS e implementarlo como un clúster de tres nodos, similar al ejemplo relecloud.com en la siguiente imagen. Esto requiere tres cmdlets de PowerShell:

- Para agregar el rol HGS, use `Install-WindowsFeature` 
- Para instalar el HGS, use `Install-HgsServer` 
- Para inicializar el HGS con el modo de atestación elegido, utilice `Initialize-HgsServer` 

Si los servidores de Hyper-V existentes no satisfacen los requisitos previos para el modo TPM (por ejemplo, no tienen TPM 2.0), puede inicializar con la atestación basada en la administración (modo de AD), que requiere una confianza de Active Directory con el dominio de fabric HGS. 

En nuestro ejemplo, supongamos que Contoso inicialmente se implementa en modo de AD con el fin de cumplir los requisitos de cumplimiento y tiene previsto convertir en la atestación basada en TPM más segura después de que se puede adquirir hardware de servidor adecuado. 

![Instalar HGS](../media/Guarded-Fabric-Shielded-VM/guarded-fabric-deployment-step-two-deploy-hgs.png)

## <a name="step-3-extract-identities-hardware-baselines-and-code-integrity-policies"></a>Paso 3: Extraer las identidades, las líneas de base de hardware y las directivas de integridad de código

El proceso para extraer las identidades de los hosts de Hyper-V depende del modo de atestación que esté utilizando.

Para el modo de AD, el identificador del host es su cuenta de equipo unido al dominio, que debe ser un miembro de un grupo de seguridad designado en el dominio de fabric.
Pertenencia al grupo designado es la única determinación de si el host es correcto o no. 

En este modo, el administrador del tejido es el único responsable de garantizar el estado de los hosts de Hyper-V. Puesto que HGS no representa ningún papel en decidir lo que es o no se puede ejecutar, malware y depuradores funcionarán según lo previsto. 

Sin embargo, se bloquean los depuradores que intentan conectarse directamente a un proceso (por ejemplo, WinDbg.exe) para máquinas virtuales blindadas porque el proceso de trabajo de la máquina virtual (VMWP.exe) es una luz de proceso protegido (PPL).
Técnicas de depuración de alternativas, como las utilizadas por LiveKd.exe, no se bloquean. A diferencia de las máquinas virtuales blindadas, el proceso de trabajo para máquinas virtuales admitido el cifrado no se ejecuta como un PPL los depuradores tradicionales como WinDbg.exe seguirá funcionando con normalidad.

Expresado de otra forma, los pasos de validación rigurosas utilizados para el modo TPM no se usan para el modo de AD de ninguna manera.

Para el modo TPM, se requieren tres cosas: 

1.  Un _clave de aprobación pública_ (o _EKpub_) desde la versión 2.0 de TPM en cada host de Hyper-V. Para capturar el EKpub, utilice `Get-PlatformIdentifier`. 
2.  Un _baseline hardware_. Si cada uno de los hosts de Hyper-V es idéntico, una sola línea de base es todo lo que necesita. Si no lo son, a continuación, necesitará una para cada clase de hardware. Es la línea de base en forma de un archivo de registro de Trustworthy Computing Group o TCGlog. El TCGlog contiene todo lo que hizo el host, desde el firmware UEFI, a través del kernel, hasta donde arrancar por completo el host. Para capturar la instantánea de hardware, instale el rol Hyper-V y la característica de compatibilidad de Hyper-V de guardián de Host y usar `Get-HgsAttestationBaselinePolicy`. 
3.  Un _directiva de integridad de código_. Si cada uno de los hosts de Hyper-V es idéntico, una única directiva de CI es todo lo que necesita. Si no lo son, a continuación, necesitará una para cada clase de hardware. Windows Server 2016 y Windows 10 tienen un nuevo formulario de cumplimiento de directivas de CI, llamado _integridad de código aplicada por hipervisor (HVCI)_. HVCI proporciona seguros de cumplimiento y garantiza que solo se permite un host para ejecutar los archivos binarios que un administrador de confianza ha permitido que se ejecute. Estas instrucciones se encapsulan en una directiva de CI que se agrega a HGS. HGS mide la directiva de CI de cada host antes de que están autorizados a ejecutar máquinas virtuales blindadas. Para capturar una directiva de CI, utilice `New-CIPolicy`. La directiva, a continuación, se debe convertir en su formato binario utilizando `ConvertFrom-CIPolicy`.

![Extraer las identidades, instantánea y directiva de CI](../media/Guarded-Fabric-Shielded-VM/guarded-fabric-deployment-step-three-extract-identity-baseline-ci-policy.png)

Eso es todo: se ha creado el tejido protegido, en términos de la infraestructura para ejecutarlo.  
Ahora puede crear un disco de plantilla de máquina virtual blindado y un archivo de datos de blindaje blindadas por lo que se pueden aprovisionar máquinas virtuales de forma sencilla y segura. 

## <a name="step-4-create-a-template-for-shielded-vms"></a>Paso 4: Crear una plantilla para las máquinas virtuales blindadas

Una plantilla de máquina virtual blindada protege los discos de plantilla mediante la creación de una firma del disco en un punto de confianza conocido en el tiempo. 
Si el disco de plantilla más adelante se infectados por malware, su firma diferirán plantilla original que se detectará la VM blindadas seguro el proceso de aprovisionamiento. 
Los discos de plantilla blindado se crean mediante la ejecución de la **Asistente de creación de disco de plantilla blindada** o `Protect-TemplateDisk` en un disco de plantilla Normal. 

Cada uno de ellos se incluye con el **herramientas VM blindadas** de características en el [remoto Server Administration Tools for Windows 10](https://www.microsoft.com/download/details.aspx?id=45520).
Después de descargar las RSAT, ejecute este comando para instalar el **herramientas VM blindadas** característica:

```powershell
Install-WindowsFeature RSAT-Shielded-VM-Tools -Restart
```

Un administrador de confianza, como el administrador del tejido o el propietario de la máquina virtual, necesitará un certificado (a menudo proporcionado por un proveedor de servicios de hospedaje) para firmar el disco de plantilla VHDX. 

![Asistente de disco de plantilla blindada](../media/Guarded-Fabric-Shielded-VM/guarded-fabric-shielded-template-wizard.png)

Se calcula la firma de disco a través de la partición del sistema operativo del disco virtual.
Si cambia algo en la partición del sistema operativo, también cambiará la firma.
Esto permite a los usuarios identificar encarecidamente que los discos que confían especificando la firma apropiada.

Revise el [los requisitos de disco de plantilla](guarded-fabric-create-a-shielded-vm-template.md) antes de comenzar. 

## <a name="step-5-create-a-shielding-data-file"></a>Paso 5: Crear un archivo de datos de blindaje 

Un archivo de datos blindaje, también conocido como un archivo. PDK, captura información importante acerca de la máquina virtual, como la contraseña de administrador. 

![Los datos de blindaje](../media/Guarded-Fabric-Shielded-VM/guarded-fabric-deployment-step-five-create-shielding-data.png)

El archivo de datos de blindaje también incluye la configuración de directiva de seguridad de la máquina virtual blindada. Debe elegir una de las dos directivas de seguridad cuando se crea un archivo de datos de blindaje:

- Blindada
   
    La opción más segura, lo que elimina muchos vectores de ataque administrativas.

- Cifrado admitido

    Un menor nivel de protección que sigue proporciona las ventajas de cumplimiento de poder cifrar una máquina virtual, pero permite a los administradores de Hyper-V hacer cosas como usar PowerShell Direct y conexión de la consola de máquina virtual. 

    ![Admite el cifrado de la nueva máquina virtual](../media/Guarded-Fabric-Shielded-VM/guarded-fabric-new-shielded-vm.png)

Puede agregar partes de administración opcionales, como VMM o Windows Azure Pack. Si desea crear una máquina virtual sin necesidad de instalar estas partes, vea [paso a paso: creación de máquinas virtuales blindadas sin VMM](https://blogs.technet.microsoft.com/datacentersecurity/2016/06/06/step-by-step-creating-shielded-vms-without-vmm/).

## <a name="step-6-create-a-shielded-vm"></a>Paso 6: Crear una máquina virtual blindada

Creación de máquinas virtuales blindadas difiere muy poco desde máquinas virtuales normales. En Windows Azure Pack, la experiencia es incluso más sencilla que crear una VM normal porque solo se debe proporcionar un nombre, archivo de datos (que contiene el resto de la información de especialización) y la red de VM de blindaje. 

![Nueva máquina virtual blindada en Windows Azure Pack](../media/Guarded-Fabric-Shielded-VM/guarded-fabric-new-vm-config.png)

## <a name="next-step"></a>Paso siguiente

>[!div class="nextstepaction"]
[Requisitos previos HGS](guarded-fabric-prepare-for-hgs.md)
