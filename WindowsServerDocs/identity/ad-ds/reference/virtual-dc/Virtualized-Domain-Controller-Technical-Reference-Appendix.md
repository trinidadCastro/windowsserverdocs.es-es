---
ms.assetid: 73a4deba-7da6-4eae-8fdd-2a4d369f9cbb
title: "Apéndice de referencia técnica de controlador de dominio virtualizada"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 2e7f264a098b6f67d98c9aa47ec5794374b8920d
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2017
---
# <a name="virtualized-domain-controller-technical-reference-appendix"></a>Apéndice de referencia técnica de controlador de dominio virtualizada

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Este tema trata:  
  
-   [Terminología](../../../ad-ds/reference/virtual-dc/../../../ad-ds/reference/virtual-dc/Virtualized-Domain-Controller-Technical-Reference-Appendix.md#BKMK_Terms)  
  
-   [FixVDCPermissions.ps1](../../../ad-ds/reference/virtual-dc/../../../ad-ds/reference/virtual-dc/Virtualized-Domain-Controller-Technical-Reference-Appendix.md#BKMK_FixPDCPerms)  
  
## <a name="BKMK_Terms"></a>Terminología  
  
-   **Instantánea** -el estado de una máquina virtual en un momento determinado. Son dependiente en la cadena de instantáneas anteriores tomadas, en el hardware y en la plataforma de virtualización.  
  
-   **Clone** - A completar y separa la copia de una máquina virtual. Depende del hardware virtual (hipervisor).  
  
-   **Completa Clone** -una completa clone es una copia independiente de una máquina virtual que no comparte ningún recurso con la máquina virtual principal después de la operación de clonación. La operación en curso de una copia completa es totalmente independiente de la máquina virtual de elemento primario.  
  
-   **Disco de diferenciación** -una copia de una máquina virtual que comparte discos virtuales con la máquina virtual de elemento primario de manera continua. Por lo general, esto ahorra espacio en disco y permite que varias máquinas virtuales para usar la misma instalación de software.  
  
-   **Copia de la máquina virtual**: un archivo de copia de sistema de todos los archivos relacionados y las carpetas de una máquina virtual.  
  
-   **Copia de archivos VHD** -una copia de VHD de una máquina virtual  
  
-   **Id. de generación de VM** : un entero de 128 bits asignado a la máquina virtual con el hipervisor. Este identificador se almacena en la memoria y restablecer cada vez que se aplica una instantánea. El diseño usa un mecanismo de hipervisor independientes para el identificador de generación de la máquina virtual en la máquina virtual de superficie. La implementación de Hyper-V expone el identificador de la tabla ACPI de la máquina virtual.  
  
-   **Importar o exportar** -característica A Hyper-V que permite al usuario guardar todo el equipo de virtual (VM archivos VHD y la configuración del equipo). Permite a los usuarios con ese conjunto de archivos para poner el equipo en el mismo equipo que la máquina virtual mismo (restauración), a continuación, en un equipo diferente, como la máquina virtual mismo (mover), o una nueva máquina virtual (copia)  
  
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
  


