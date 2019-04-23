---
ms.assetid: 73a4deba-7da6-4eae-8fdd-2a4d369f9cbb
title: Apéndice de la referencia técnica de controladores de dominio virtualizados
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 9e3a5cc2c71455bb040f1311bdbfed1ac7e213fb
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59832236"
---
# <a name="virtualized-domain-controller-technical-reference-appendix"></a>Apéndice de la referencia técnica de controladores de dominio virtualizados

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

En esta sección se tratan los siguientes temas:  
  
-   [Terminología](../../../ad-ds/reference/virtual-dc/../../../ad-ds/reference/virtual-dc/Virtualized-Domain-Controller-Technical-Reference-Appendix.md#BKMK_Terms)  
  
-   [FixVDCPermissions.ps1](../../../ad-ds/reference/virtual-dc/../../../ad-ds/reference/virtual-dc/Virtualized-Domain-Controller-Technical-Reference-Appendix.md#BKMK_FixPDCPerms)  
  
## <a name="BKMK_Terms"></a>Terminología  
  
-   **Instantánea** : el estado de una máquina virtual en un momento determinado. Es dependiente de la cadena de las instantáneas anteriores realizadas, en el hardware y en la plataforma de virtualización.  
  
-   **Clon** - A completar y separar la copia de una máquina virtual. Depende en el hardware (hipervisor).  
  
-   **Completa la clonación** -un clon completo es una copia independiente de una máquina virtual que no comparte ningún recurso con la máquina virtual principal después de la operación de clonación. Operación en curso de un clon completo es completamente independiente de la máquina virtual principal.  
  
-   **Disco de diferenciación** -una copia de una máquina virtual que comparte los discos virtuales con la máquina virtual principal en un modo continuo. Normalmente, esto ahorra espacio en disco y permite que varias máquinas virtuales usar la misma instalación de software.  
  
-   **Copia de la máquina virtual**- un archivo de copia de todos los archivos relacionados del sistema y las carpetas de una máquina virtual.  
  
-   **Copia de archivos de disco duro virtual** -una copia del VHD de una máquina virtual  
  
-   **Id. de generación de VM** : un entero de 128 bits asignada a la máquina virtual por el hipervisor. Este identificador se almacena en memoria y restablece cada vez que se aplica una instantánea. Este diseño utiliza un mecanismo independiente del hipervisor para exponer el identificador de generación de VM en la máquina virtual. La implementación de Hyper-V expone el identificador de la tabla ACPI de la máquina virtual.  
  
-   **Import/Export** -característica A Hyper-V que permite al usuario guardar la máquina virtual completa (archivos VM, disco duro virtual y la configuración del equipo). Permite que los usuarios usando ese conjunto de archivos para poner el equipo en el mismo equipo que la misma máquina virtual (restauración), a continuación, en un equipo diferente, como la misma máquina virtual (mover) o una nueva máquina virtual (copia)  
  
## <a name="BKMK_FixPDCPerms"></a>FixVDCPermissions.ps1  
  
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
  


