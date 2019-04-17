---
title: "Recuperación de bosque de AD - generar eliminar grupos"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 07/07/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: c37bc129-a5e0-4219-9ba7-b4cf3a9fc9a4
ms.technology: identity-adfs
ms.openlocfilehash: e6b5dc8b9c0b701fe2cd1b0c88f7edc22802c393
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2017
---
# <a name="ad-forest-recovery---raising-the-value-of-available-rid-pools"></a>Recuperación de bosque de AD - generar el valor de grupos RID disponibles 

>Se aplica a: Windows Server 2016, Windows Server 2012 y 2012 R2, Windows Server 2008 y 2008 R2
 
 Usa el siguiente procedimiento para generar el valor de identificador relativo (RID) de grupos que asigna el maestro de operaciones RID después de restaura ese controlador de dominio. Generando el valor de los grupos de RID disponibles, puedes asegurarte de que ningún controlador de dominio asigna un RID para una entidad de seguridad que se creó después de la copia de seguridad que se usó para restaurar el dominio.  
 
## <a name="about-active-directory-rid-pools-and-ridavailablepool"></a>Acerca de los grupos de LIBRARSE de Active Directory y rIDAvailablePool
 Cada dominio tiene un objeto **CN = Administrador RID $, CN = sistema, DC**=<*nombreDeDominio*>. Este objeto tiene un atributo denominado **rIDAvailablePool**. Este valor de atributo mantiene el espacio RID globales para todo un dominio. El valor es un entero de gran tamaño con partes superior e inferior. La parte superior, define el número de entidades de seguridad que se puede asignar para cada dominio (0x3FFFFFFF o simplemente más de mil millones de 1). La parte inferior es el número de RID que se han asignado en el dominio.  
  
> [!NOTE]
>  En Windows Server 2016 y 2012, se incrementa el número de entidades de seguridad que se puede asignar a más millones de 2. Para obtener más información, consulta [administrar LIBRARSE emisión](https://technet.microsoft.com/library/jj574229.aspx).  
  
-   El valor de muestra: 4611686014132422708  
  
-   Parte baja: 2100 (a partir de la siguiente conjunto de RID asignarse)  
  
-   La parte superior: 1073741823 (número total de RID que pueden crearse en un dominio)  
  
 Cuando se aumenta el valor de los números enteros grandes, aumenta el valor de la parte baja. Por ejemplo, si agregas 100.000 en el valor de muestra de 4611686014132422708 para una suma de 4611686014132522708, la parte baja nuevo es 102100. Esto indica que el siguiente conjunto de RID que se asignará el maestro RID comenzará con 102100 en lugar de 2100.  
  
### <a name="to-raise-the-value-of-available-rid-pools-using-adsiedit-and-the-calculator--"></a>Para generar el valor de grupos RID disponibles con la calculadora y adsiedit '  
1.  Abre el administrador del servidor, haz clic en **herramientas** y haz clic en **ADSIEdit**.    
2.  Con el botón derecho, selecciona **conectarse** y conectar el contexto de nomenclatura predeterminado y haga clic en **Aceptar**.
![Editor ADSI](media/AD-Forest-Recovery-Raise-RID-Pool/adsi1.png) 
3. Vaya a la siguiente ruta de acceso completa: **CN = Administrador RID $, CN = sistema, DC =<domain name>**.
![Editor ADSI](media/AD-Forest-Recovery-Raise-RID-Pool/adsi2.png) 
3.  Haz clic en y y selecciona las propiedades de CN = Administrador RID $.  
4.  Selecciona el atributo **rIDAvailablePool**, haz clic en **editar**y, a continuación, copia el valor de números enteros grandes en el Portapapeles.
![Editor ADSI](media/AD-Forest-Recovery-Raise-RID-Pool/adsi3.png)  
5.  Iniciar la calculadora y desde el **vista** menú, selecciona **el modo científica**.  6.  Agregar 100.000 al valor actual.  
![Editor ADSI](media/AD-Forest-Recovery-Raise-RID-Pool/adsi4.png) 
7.  Con ctrl-c, o la **copia** comando desde el **editar** menú, copiar el valor en el Portapapeles.  
8.  En el cuadro de diálogo de edición de adsiedit, pega este nuevo valor. 
![Editor ADSI](media/AD-Forest-Recovery-Raise-RID-Pool/adsi5.png) 
9. Haz clic en **Aceptar** en el cuadro de diálogo y **aplicar** en la hoja de propiedades para actualizar la **rIDAvailablePool** atributo.  
  
### <a name="to-raise-the-value-of-available-rid-pools-using-ldp"></a>Para generar el valor de grupos RID mediante LDP  
  
1.  En el símbolo del sistema, escribe el siguiente comando y, a continuación, presiona ENTRAR:  
  
     **Ldp**  
  
2.  Haz clic en **conexión**, haz clic en **conectar**, escribe el nombre del Administrador de RID y, a continuación, haz clic en **Aceptar **.  
![LDP](media/AD-Forest-Recovery-Raise-RID-Pool/ldp1.png)
3.  Haz clic en **conexión**, haz clic en **enlazar**, selecciona **enlazar con credenciales** y escribe las credenciales administrativas y, a continuación, haz clic en **Aceptar **.  
![LDP](media/AD-Forest-Recovery-Raise-RID-Pool/ldp2.png)
4.  Haz clic en **vista**, haz clic en **árbol** y, a continuación, escribe la siguiente ruta de acceso completa: CN = Administrador RID $, CN = sistema, DC =*nombre de dominio*  
![LDP](media/AD-Forest-Recovery-Raise-RID-Pool/ldp3.png)
5.  Haz clic en **examinar**y, a continuación, haz clic en **modificar **.  
6.  Agregar 100.000a los valores actuales **rIDAvailablePool** valor y, a continuación, escribe la suma en **valores **.  
7.  En **Dn**, tipo `cn=RID Manager$,cn=System,dc=`*< dominio equipo\ >*.  
8.  En **editar el atributo de entrada**, tipo `rIDAvailablePool`.  
9. Selecciona **reemplazar** como de la operación y, a continuación, haz clic en **ENTRAR **. </br>
![LDP](media/AD-Forest-Recovery-Raise-RID-Pool/ldp4.png) 
10. Haz clic en **ejecutar** para ejecutar la operación.  Haz clic en **cerrar **.
11. Para validar el cambio, haz clic en **vista**, haz clic en **árbol**y, a continuación, escribe la siguiente ruta de acceso completa: CN = Administrador RID $, CN = sistema, DC =*nombre de dominio *.    Comprobar la **rIDAvailablePool** atributo.  
![LDP](media/AD-Forest-Recovery-Raise-RID-Pool/ldp5.png)

## <a name="next-steps"></a>Pasos siguientes

- [Guía de recuperación del bosque de AD](AD-Forest-Recovery-Guide.md)
- [Recuperación de bosque de AD - procedimientos](AD-Forest-Recovery-Procedures.md)
 
