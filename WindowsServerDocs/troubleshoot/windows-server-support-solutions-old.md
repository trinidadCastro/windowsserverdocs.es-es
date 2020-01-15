---
title: Soluciones principales de soporte técnico para Windows Server
description: Obtener vínculos a soluciones para problemas de Windows Server
ms.prod: windows-server
ms.service: na
manager: alant
ms.technology: server-general
ms.date: 03/16/2018
ms.topic: article
author: kaushika-msft
ms.author: elizapo
ms.openlocfilehash: 61c10f25ac97934f73c4f393e2c91c9b36fc59fd
ms.sourcegitcommit: 083ff9bed4867604dfe1cb42914550da05093d25
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/14/2020
ms.locfileid: "75950175"
---
# <a name="top-support-solutions-for-windows-server-2016"></a>Soluciones principales de soporte técnico para Windows Server 2016

Microsoft publica periódicamente actualizaciones y soluciones para Windows Server. Para garantizar que los servidores pueden recibir actualizaciones futuras, incluidas las actualizaciones de seguridad, es importante mantenerlos actualizados. Echa un vistazo a [Historial de actualizaciones de Windows 10 y Windows Server 2016](https://support.microsoft.com/help/4000825/windows-10-windows-server-2016-update-history) para obtener una lista completa de las actualizaciones publicadas.

Estas son las mejores soluciones del Soporte técnico de Microsoft para la mayoría de problemas comunes experimentados con Windows Server 2016. Los siguientes vínculos incluyen vínculos a artículos KB, actualizaciones y artículos de la biblioteca.

>[!TIP]
> ¿Busca información sobre versiones anteriores de Windows Server? Eche un vistazo a nuestras otras [bibliotecas de Windows Server](/previous-versions/windows/) en docs.microsoft.com. También puede [buscar en este sitio](https://docs.microsoft.com/search/index?search=Windows+Server&dataSource=previousVersions) para obtener información específica.

## <a name="solutions-for-installing-or-upgrading-windows-server"></a>Soluciones para la instalación o actualización de Windows Server

- [Resolver errores de actualización de Windows 10: información técnica para profesionales de ti](https://docs.microsoft.com/windows/deployment/upgrade/resolve-windows-10-upgrade-errors)
- [Actualización de la pila de mantenimiento para Windows 10 versión 1607 y Windows Server 2016:8 de agosto de 2017](https://support.microsoft.com/help/4035631)
- [Actualización de compatibilidad para actualizar a Windows 10, versión 1607 y Windows Server 2016:3 de agosto de 2017](https://support.microsoft.com/help/4033524)
- [No se admite la actualización en contexto del sistema en máquinas virtuales de Azure basadas en Windows](https://support.microsoft.com/help/4014997)
- [Upgrade and conversion options for Windows Server 2016](../get-started/supported-upgrade-paths.md) (Opciones de actualización y conversión de Windows Server 2016)
- [Matriz de actualización y migración del rol de servidor para Windows Server 2016](../get-started/server-role-upgradeability-table.md)
- [Instalación y actualización de Windows Server](../get-started/installation-and-upgrade.md)
- [Notas de la versión: problemas importantes en Windows Server 2016](../get-started/windows-server-2016-ga-release-notes.md)
- [Recomendaciones para migrar a Windows Server 2016](../get-started/recommendations-moving-to-server2016.md)

## <a name="solutions-for-volume-activation"></a>Soluciones de activación por volumen
- [Activación de Windows Server 2016](../get-started/server-2016-activation.md)
- [Review and Select Activation Methods](https://technet.microsoft.com/library/jj134256(ws.11).aspx) (Revisión y selección de métodos de activación)
- [Códigos de error de activación para activación de volumen](https://technet.microsoft.com/library/dn502528.aspx)
- [Cómo solucionar problemas del servicio de administración de claves (KMS)](https://technet.microsoft.com/library/ee939272.aspx)
- [Solución de problemas de activación de volumen](https://technet.microsoft.com/library/ff793439.aspx)
- [Códigos de error de activación](https://technet.microsoft.com/library/ff793399.aspx)
- [La instalación de Windows puede producir el error "la clave de producto especificada no coincide con ninguna de las imágenes de Windows disponibles para la instalación. Escriba una clave de producto diferente "](https://support.microsoft.com/help/2796988/windows-8-or-windows-server-2012-installation-may-fail-with-error-mess)

## <a name="solutions-related-to-dcpromo-and-installing-domain-controllers"></a>Soluciones relacionados con DCPromo y la instalación de controladores de dominio
- [Requisitos de puertos Active Directory y Active Directory Domain Services](https://technet.microsoft.com/library/dd772723(v=ws.10).aspx)
- [Active Directory puertos de Firewall: vamos a intentar hacer esto sencillo](http://blogs.msmvps.com/acefekay/2011/11/01/active-directory-firewall-ports-let-s-try-to-make-this-simple/)
- [Compatibilidad de Exchange Server con Windows Server 2016](https://technet.microsoft.com/library/ff728623(v=exchg.150).aspx)
- [Usar Ntdsutil. exe para transferir o asumir roles FSMO en un controlador de dominio](https://support.microsoft.com/kb/255504)
- [Solución de problemas de implementación de controladores de dominio](../identity/ad-ds/deploy/troubleshooting-domain-controller-deployment.md)
- [Solución de problemas del asistente para la instalación de Active Directory](https://msdn.microsoft.com/library/bb727058.aspx)
- [Problemas conocidos de la instalación y eliminación de AD DS](https://technet.microsoft.com/library/cc754463(v=ws.10).aspx)

## <a name="solutions-for-active-directory-federation-services-ad-fs"></a>Soluciones de Servicios de federación de Active Directory (AD FS)
- [Configuración del registro automático de dispositivos unidos a un dominio de Windows con Azure Active Directory](/azure/active-directory/active-directory-conditional-access-automatic-device-registration-setup)
- [Configuración de la emisión de notificaciones](/azure/active-directory/device-management-hybrid-azuread-joined-devices-setup#step-2-setup-issuance-of-claims)
- [Configuración de AD FS para autenticar a los usuarios almacenados en directorios LDAP](../identity/ad-fs/operations/configure-ad-fs-to-authenticate-users-stored-in-ldap-directories.md)
- [Compatibilidad de AD FS con el enlace de nombre de host alternativo para la autenticación de certificado](../identity/ad-fs/operations/ad-fs-support-for-alternate-hostname-binding-for-certificate-authentication.md)
- [Protección contra ataques de contraseñas](https://blogs.technet.microsoft.com/tspring/2017/01/20/federated-to-microsoft-cloud-and-account-lockouts/)
- [Actualización a AD FS en Windows Server 2016 mediante una base de datos WID](../identity/ad-fs/deployment/upgrading-to-ad-fs-in-windows-server-2016.md)
- [Inicio de sesión de Windows 10: habilitar la autenticación de dispositivos con AD FS](../identity/ad-fs/operations/configure-device-based-conditional-access-on-premises.md)
- [Administración de certificados SSL en AD FS y WAP en Windows Server 2016](../identity/ad-fs/operations/manage-ssl-certificates-ad-fs-wap-2016.md)
- [Directivas de Access Control en Windows Server 2016 AD FS](../identity/ad-fs/operations/access-control-policies-in-ad-fs.md)

## <a name="solutions-related-to-active-directory-replication"></a>Soluciones relacionadas con la replicación de Active Directory

- [Solución de problemas de replicación de Active Directory](../identity/ad-ds/manage/troubleshoot/troubleshooting-active-directory-replication-problems.md)
- [Descargar Active Directory Replication Status herramienta del centro de descarga de Microsoft](https://www.microsoft.com/en-in/download/details.aspx?id=30005)
- [E2E: Cómo solucionar errores comunes de replicación de Active Directory](https://support.microsoft.com/kb/3108513)
- [Solución de problemas de replicación de AD 8606: se asignaron atributos insuficientes para crear un objeto](https://support.microsoft.com/kb/2028495)
- [El ID. de evento 2108 y el ID. de evento 1084 se producen durante la replicación de entrada de Active Directory en el servidor de Windows 2000 y en Windows Server 2003](https://support.microsoft.com/kb/837932)
- [Solución de problemas de replicación de AD 8451: la operación de replicación encontró un error de base de datos](https://support.microsoft.com/kb/2645996)
- [Solución de problemas de replicación de AD 1127: al tener acceso al disco duro, se produjo un error en una operación de disco incluso después de reintentos](https://support.microsoft.com/kb/2025726)
- [Limpiar metadatos de servidor](https://technet.microsoft.com/library/cc816907.aspx)