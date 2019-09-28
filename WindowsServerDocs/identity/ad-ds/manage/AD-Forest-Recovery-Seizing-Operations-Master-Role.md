---
title: 'Recuperación de bosque de AD: Asunción de un rol de maestro de operaciones'
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server
ms.assetid: 7e6bb370-f840-4416-b5e2-86b0ba715f4f
ms.technology: identity-adds
ms.openlocfilehash: 672dc119845acbe9cf38f82c793bd377d31db3b2
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71390278"
---
# <a name="ad-forest-recovery---seizing-an-operations-master-role"></a>Recuperación de bosque de AD: Asunción de un rol de maestro de operaciones  

>Se aplica a: Windows Server 2016, Windows Server 2012 y 2012 R2, Windows Server 2008 y 2008 R2

Utilice el procedimiento siguiente para asumir un rol de maestro de operaciones (también conocido como rol de operaciones de maestro único flexible (FSMO)). Puede usar Ntdsutil. exe, una herramienta de línea de comandos que se instala automáticamente en todos los controladores de sesión.  
  
## <a name="to-seize-an-operations-master-role"></a>Para asumir una función de maestro de operaciones  
  
1. En el símbolo del sistema, escriba el siguiente comando y presione ENTRAR:  

   ```  
   ntdsutil  
   ```  

2. En el símbolo del sistema **Ntdsutil:** , escriba el siguiente comando y, a continuación, presione ENTRAR:  

   ```  
   roles  
   ```  

3. En el símbolo del sistema **FSMO Maintenance:** , escriba el siguiente comando y, a continuación, presione ENTRAR:  

   ```  
   connections  
   ```  

4. En el cuadro de texto **conexiones del servidor:** , escriba el siguiente comando y, a continuación, presione ENTRAR:  

   ```  
   Connect to server ServerFQDN  
   ```  

   Donde *ServerFQDN* es el nombre de dominio completo (FQDN) de este DC, por ejemplo: **connect to Server nycdc01.example.com**.  

   Si *ServerFQDN* no se ejecuta correctamente, utilice el nombre NetBIOS del controlador de dominio.  

5. En el cuadro de texto **conexiones del servidor:** , escriba el siguiente comando y, a continuación, presione ENTRAR:  

   ```  
   quit  
   ```  

6. En función del rol que desee asumir, en el símbolo del sistema **FSMO Maintenance:** , escriba el comando adecuado como se describe en la tabla siguiente y, a continuación, presione Entrar.  
  
|Rol|Credenciales|Comando|  
|----------|-----------------|-------------|  
|Maestro de nomenclatura de dominios|Administradores de empresas|**Asunción del maestro de nomenclatura**|  
|Maestro de esquema|Administradores de esquema|**Asumir el maestro de esquema**|  
|Nota del maestro de infraestructura **:**  Después de asumir el rol de maestro de infraestructura, es posible que reciba un error más adelante si necesita ejecutar Adprep/rodcprep. Para obtener más información, consulte el artículo [949257](https://support.microsoft.com/kb/949257)de Knowledge base.|Admins. del dominio|**Asumir el maestro de infraestructura**|  
|Maestro de emulador de PDC|Admins. del dominio|**Asunción de PDC**|  
|Maestro de RID de|Admins. del dominio|**Asumir el maestro RID**|  

Después de confirmar la solicitud, Active Directory o AD DS intenta transferir el rol. Cuando se produce un error en la transferencia, aparece cierta información de error y Active Directory o AD DS continúa con el embargo. Una vez completada la Asunción, se muestra una lista de los roles y el nombre del Protocolo ligero de acceso a directorios (LDAP) del servidor que tiene actualmente cada rol. También puede ejecutar **netdom query fsmo** en un símbolo del sistema con privilegios elevados para comprobar los titulares de la función actual.  
  
> [!NOTE]
> Si este equipo no era un maestro de RID antes del error e intenta asumir el rol de maestro de RID, el equipo intentará sincronizar con un asociado de replicación antes de aceptar este rol. Sin embargo, dado que este paso se realiza cuando el equipo está aislado, no se realizará correctamente la sincronización con un socio. Por lo tanto, aparece un cuadro de diálogo que le pregunta si desea continuar con la operación a pesar de que este equipo no se puede sincronizar con un socio comercial. Haga clic en **Sí**.  
  
## <a name="next-steps"></a>Pasos siguientes

- [Guía de recuperación del bosque de AD](AD-Forest-Recovery-Guide.md)
- [Recuperación del bosque de AD: procedimientos](AD-Forest-Recovery-Procedures.md)
