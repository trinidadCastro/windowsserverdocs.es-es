---
title: "Recuperación de bosque de AD - asumir un rol de maestro de operaciones"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 07/07/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: 7e6bb370-f840-4416-b5e2-86b0ba715f4f
ms.technology: identity-adfs
ms.openlocfilehash: 7ca6e3746586feeb3573b1ad6ba02831d1f5addd
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2017
---
# <a name="ad-forest-recovery---seizing-an-operations-master-role"></a>Recuperación de bosque de AD - asumir un rol de maestro de operaciones  

>Se aplica a: Windows Server 2016, Windows Server 2012 y 2012 R2, Windows Server 2008 y 2008 R2

 Usa el siguiente procedimiento para asumir un rol de maestro de operaciones (también conocida como una función de operaciones de maestro único flexible (FSMO)). Puedes usar Ntdsutil.exe, una herramienta de línea de comandos que se instalen automáticamente en todos los controladores de dominio.  
  
## <a name="to-seize-an-operations-master-role"></a>Para asumir un rol de maestro de operaciones  
  
1.  En el símbolo del sistema, escribe el siguiente comando y, a continuación, presiona ENTRAR:  
  
    ```  
    ntdsutil  
    ```  
  
2.  En la **ntdsutil:** pedir, escribe el siguiente comando y, a continuación, presiona ENTRAR:  
  
    ```  
    roles  
    ```  
  
3.  En la **mantenimiento FSMO:** pedir, escribe el siguiente comando y, a continuación, presiona ENTRAR:  
  
    ```  
    connections  
    ```  
  
4.  En la **conexiones de servidor:** pedir, escribe el siguiente comando y, a continuación, presiona ENTRAR:  
  
    ```  
    Connect to server ServerFQDN  
    ```  
  
     Donde *FQDNdeServidor* es el nombre de dominio completo (FQDN) este controlador de dominio, por ejemplo: **conectarse al servidor nycdc01.example.com**.  
  
     Si *FQDNdeServidor* no se realice correctamente, usa el nombre NetBIOS del controlador de dominio.  
  
5.  En la **conexiones de servidor:** pedir, escribe el siguiente comando y, a continuación, presiona ENTRAR:  
  
    ```  
    quit  
    ```  
  
6.  Dependiendo de la función que quieres asumir, en la **mantenimiento FSMO:** pedir, escribe el comando adecuado, tal como se describe en la tabla siguiente y, a continuación, presione ENTRAR.  
  
    |Función|Credenciales|Comando|  
    |----------|-----------------|-------------|  
    |Patrón de nomenclatura de dominio|Administradores de empresa|**Asumir el patrón de nomenclatura**|  
    |Maestro de esquema|Administradores de esquema|**Asumir a maestro de esquema**|  
    |Maestro de infraestructura **Nota:** después asumir la función de maestro de infraestructura, recibirá un error más adelante si necesitas ejecutar Adprep /Rodcprep. Para obtener más información, consulte el artículo [949257](https://support.microsoft.com/kb/949257).|Administradores de dominio|**Asumir a maestro de infraestructura**|  
    |Emulador PDC|Administradores de dominio|**Asumir pdc**|  
    |QUITAR maestro|Administradores de dominio|**Asumir deshacerse de maestro**|  
  
     Después de confirmar la solicitud, intenta transferir la función de Active Directory o AD DS. Cuando se produce un error en la transferencia, cierta información de error aparece y ganancias asumir la de Active Directory o AD DS. Una vez completada la asunción, aparece una lista de las funciones y el nombre de protocolo ligero de acceso a directorios (LDAP) del servidor que contiene actualmente cada rol. También puedes ejecutar **Netdom Query FSMO** en un símbolo del sistema con privilegios elevados para comprobar el titular de una función actual.  
  
    > [!NOTE]
    >  Si este equipo no era un maestro RID antes del error e intenta asumir la función de maestro RID, el equipo intenta sincronizar con un duplicador antes de aceptar este rol. Sin embargo, ya que este paso se realiza cuando el equipo está aislado, no se podrán sincronizar con un compañero. Por lo tanto, aparecerá un cuadro de diálogo que pregunta si quieres continuar con la operación a pesar de que este equipo no se podrán sincronizar con un compañero. Haz clic en **Sí**.  
  
## <a name="next-steps"></a>Pasos siguientes

- [Guía de recuperación del bosque de AD](AD-Forest-Recovery-Guide.md)
- [Recuperación de bosque de AD - procedimientos](AD-Forest-Recovery-Procedures.md)
