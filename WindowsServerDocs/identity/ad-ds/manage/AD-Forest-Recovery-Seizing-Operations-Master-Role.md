---
title: Recuperación de bosques de AD - asumir una función de maestro de operaciones
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: 7e6bb370-f840-4416-b5e2-86b0ba715f4f
ms.technology: identity-adds
ms.openlocfilehash: 1994d49652ee9eb10f6afc73cf5b4630b4718e77
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59858466"
---
# <a name="ad-forest-recovery---seizing-an-operations-master-role"></a>Recuperación de bosques de AD - asumir un rol de maestro de operaciones  

>Se aplica a: Windows Server 2016, Windows Server 2012 y 2012 R2, Windows Server 2008 y 2008 R2

Use el procedimiento siguiente para asumir una función de maestro de operaciones (también conocido como un rol de operaciones de maestro único flexible (FSMO)). Puede usar Ntdsutil.exe, una herramienta de línea de comandos que se instala automáticamente en todos los controladores de dominio.  
  
## <a name="to-seize-an-operations-master-role"></a>Asumir una función de maestro de operaciones  
  
1. En el símbolo del sistema, escriba el siguiente comando y presione ENTRAR:  

   ```  
   ntdsutil  
   ```  

2. En el **ntdsutil:** símbolo del sistema, escriba el siguiente comando y, a continuación, presione ENTRAR:  

   ```  
   roles  
   ```  

3. En el **mantenimiento FSMO:** símbolo del sistema, escriba el siguiente comando y, a continuación, presione ENTRAR:  

   ```  
   connections  
   ```  

4. En el **las conexiones del servidor:** símbolo del sistema, escriba el siguiente comando y, a continuación, presione ENTRAR:  

   ```  
   Connect to server ServerFQDN  
   ```  

   Donde *ServerFQDN* es el nombre de dominio completo (FQDN) de este controlador de dominio, por ejemplo: **conectarse al servidor nycdc01.example.com**.  

   Si *ServerFQDN* no se realice correctamente, use el nombre de NetBIOS del controlador de dominio.  

5. En el **las conexiones del servidor:** símbolo del sistema, escriba el siguiente comando y, a continuación, presione ENTRAR:  

   ```  
   quit  
   ```  

6. Dependiendo de la función que desea asumir, en el **mantenimiento FSMO:** símbolo del sistema, escriba el comando adecuado como se describe en la tabla siguiente y, a continuación, presione ENTRAR.  
  
|Rol|Credenciales|Comando|  
|----------|-----------------|-------------|  
|Maestro de nomenclatura de dominio|Administradores de empresas|**Asumir el maestro de nomenclatura**|  
|Maestro de esquema|Administradores de esquema|**Asumir el maestro de esquema**|  
|Maestro de infraestructura **Nota:**  Después de asumir el rol de maestro de infraestructura, puede recibir un error más adelante si necesita ejecutar Adprep /Rodcprep. Para obtener más información, consulte el artículo de KB [949257](https://support.microsoft.com/kb/949257).|Admins. del dominio|**Asumir el maestro de infraestructura**|  
|Maestro emulador de PDC|Admins. del dominio|**Asumir pdc**|  
|Maestro de RID de|Admins. del dominio|**Asumir el maestro de rid**|  

Después de confirmar la solicitud, Active Directory o AD DS intenta transferir el rol. Cuando se produce un error en la transferencia, aparece información de error y Active Directory o AD DS continúa con la asunción. Una vez completada la asunción, aparece una lista de los roles y el nombre de protocolo ligero de acceso a directorios (LDAP) del servidor que contiene actualmente cada rol. También puede ejecutar **Netdom Query FSMO** en un símbolo del sistema con privilegios elevados para comprobar los titulares de función actual.  
  
> [!NOTE]
> Si este equipo no pudo un maestro de RID antes del error e intenta asumir el rol de maestro de RID, el equipo intenta sincronizar con un asociado de replicación antes de aceptar este rol. Sin embargo, dado que este paso se realiza cuando el equipo está aislado, no tendrán éxito en la sincronización con un socio comercial. Por lo tanto, aparece un cuadro de diálogo que pregunta si desea continuar con la operación a pesar de que este equipo no se pueda sincronizar con un asociado. Haga clic en **Sí**.  
  
## <a name="next-steps"></a>Pasos siguientes

- [Guía de recuperación de bosque de AD](AD-Forest-Recovery-Guide.md)
- [Recuperación de bosques de AD: procedimientos](AD-Forest-Recovery-Procedures.md)
