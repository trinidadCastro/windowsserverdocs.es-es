---
ms.assetid: 73a4deba-7da6-4eae-8fdd-2a4d369f9cbb
title: Apéndice de la referencia técnica de controladores de dominio virtualizados
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: e1018d5bbff5922df5a696e5c4fad12dc9f6ec3d
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71408572"
---
# <a name="virtualized-domain-controller-technical-reference-appendix"></a>Apéndice de la referencia técnica de controladores de dominio virtualizados

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

En esta sección se tratan los siguientes temas:  
  
-   [Terminología](../../../ad-ds/reference/virtual-dc/../../../ad-ds/reference/virtual-dc/Virtualized-Domain-Controller-Technical-Reference-Appendix.md#BKMK_Terms)  
  
-   [FixVDCPermissions. ps1](../../../ad-ds/reference/virtual-dc/../../../ad-ds/reference/virtual-dc/Virtualized-Domain-Controller-Technical-Reference-Appendix.md#BKMK_FixPDCPerms)  
  
## <a name="BKMK_Terms"></a>Terminología  
  
-   **Instantánea** : el estado de una máquina virtual en un momento determinado. Depende de la cadena de instantáneas anteriores tomada, en el hardware y en la plataforma de virtualización.  
  
-   **Clone** : una copia completa y independiente de una máquina virtual. Depende del hardware virtual (hipervisor).  
  
-   **Clon completo** : un clon completo es una copia independiente de una máquina virtual que no comparte recursos con la máquina virtual principal después de la operación de clonación. El funcionamiento continuo de un clon completo es completamente independiente de la máquina virtual principal.  
  
-   **Disco de diferenciación** : una copia de una máquina virtual que comparte discos virtuales con la máquina virtual principal de manera continua. Normalmente, esto ahorra espacio en disco y permite que varias máquinas virtuales usen la misma instalación de software.  
  
-   **Copia de máquina virtual**: una copia del sistema de archivos de todos los archivos y carpetas relacionados de una máquina virtual.  
  
-   **Copia de archivo VHD** : una copia del VHD de una máquina virtual  
  
-   **Identificador de generación de VM** : entero de 128 bits proporcionado a la máquina virtual por el hipervisor. Este identificador se almacena en memoria y se restablece cada vez que se aplica una instantánea. El diseño usa un mecanismo independiente del hipervisor para exponer el identificador de generación de VM en la máquina virtual. La implementación de Hyper-V expone el identificador en la tabla ACPI de la máquina virtual.  
  
-   **Import/Export** : una característica de Hyper-V que permite al usuario guardar la máquina virtual completa (archivos de VM, VHD y la configuración de la máquina). Después, permite a los usuarios usar ese conjunto de archivos para volver a poner la máquina en el mismo equipo que la misma máquina virtual (restauración), en otro equipo como la misma máquina virtual (movimiento) o en una nueva máquina virtual (copiar).  
  
## <a name="BKMK_FixPDCPerms"></a>FixVDCPermissions. ps1  
  
```  
# Unsigned script, requires use of set-executionpolicy remotesigned -force  
# You must run the Windows PowerShell console as an elevated administrator  
  
# Load Active Directory Windows PowerShell Module and switch to AD DS drive  
import-module activedirectory  
cd ad:  
  
## Get Domain NC  
$domainNC = get-addomain  
  
## Get groups and obtain their SIDs   
$dcgroup = get-adgroup "Cloneable Domain Controllers"  
  
$sid1 = (get-adgroup $dcgroup).sid  
  
## Get the DACL of the domain  
$acl = get-acl $domainNC  
  
## The following object specific ACE grants extended right 'Allow a DC to create a clone of itself' for the CDC group to the Domain NC  
## 3e0f7e18-2c7a-4c10-ba82-4d926db99a3e is the schemaIDGuid for 'DS-Clone-Domain-Controller"  
  
$objectguid = new-object Guid 3e0f7e18-2c7a-4c10-ba82-4d926db99a3e  
$ace1 = new-object System.DirectoryServices.ActiveDirectoryAccessRule $sid1,"ExtendedRight","Allow",$objectguid  
  
## Add the ACE in the ACL and set the ACL on the object   
  
$acl.AddAccessRule($ace1)  
set-acl -aclobject $acl $domainNC  
write-host "Done writing new VDC permissions."  
cd c:   
```  
  


