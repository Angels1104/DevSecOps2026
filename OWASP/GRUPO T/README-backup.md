 # OWASP TOP 10 


# Integrantes

- Bernal Arcieri Laura Vanessa
- Reynel Alfonso Cely Gomez
- Bayardo Alejandro Medina Diaz
- Johanna Ortiz Pacheco

## IntroducciÃ³n

OWASP (Open Web Application Security Project) es una organizaciÃ³n sin Ã¡nimo de lucro enfocada en mejorar la seguridad del software.
El OWASP Top 10 es un documento de concientizaciÃ³n que identifica los riesgos de seguridad mÃ¡s crÃ­ticos en aplicaciones web. Representa un amplio consenso de expertos en seguridad a nivel mundial y sirve como referencia para desarrolladores, arquitectos de software y equipos de gestiÃ³n de riesgos.
Esta clasificaciÃ³n se actualiza aproximadamente cada cuatro aÃ±os, evaluando y priorizando las vulnerabilidades mÃ¡s relevantes segÃºn su impacto y frecuencia. En el presente trabajo se analizarÃ¡n diez vulnerabilidades incluidas en el Top 10, detallando su descripciÃ³n, mÃ©todos de explotaciÃ³n y mejores prÃ¡cticas de mitigaciÃ³n.
El OWASP Top 10 no es simplemente una lista de vulnerabilidades, sino un reflejo de cÃ³mo la seguridad en el desarrollo de software evoluciona constantemente. A continuaciÃ³n, se presenta una imagen comparativa que muestra cÃ³mo las vulnerabilidades han cambiado su posiciÃ³n entre 2021 y 2025, evidenciando la actualizaciÃ³n mÃ¡s reciente realizada por la organizaciÃ³n y cÃ³mo el panorama de amenazas continÃºa transformÃ¡ndose a nivel mundial.

<p align="center">
  <img src="images/OWASPTOP10.png" width="600">
</p>

# A01:2025 â Broken Access Control
Es vulnerabilidad se lleva acabo cuando una aplicaciÃ³n no restringe adecuadamente las acciones que un usuario autenticado puede realizar, basicamente nos indica el Broken Access Control el usuario estÃ¡ autenticado, pero puede acceder a recursos o funciones que no deberÃ­a, como se evidencia en la siguiente imagen.
<p align="center">
  <img src="images/access.png" width="600">
</p>

## Naturaleza del problema

- Falta de validaciÃ³n en backend
- Validaciones solo en frontend
- Uso incorrecto de roles
- Ausencia de controles de autorizaciÃ³n


## Impacto potencial

- Acceso a datos sensibles
- Escalamiento de privilegios
- ModificaciÃ³n o eliminaciÃ³n de informaciÃ³n
- Compromiso total del sistema

## MÃ©todos de ExplotaciÃ³n

### IDOR (Insecure Direct Object Reference)

Un **IDOR** ocurre cuando una aplicaciÃ³n utiliza un identificador directo (por ejemplo, un nÃºmero de usuario o de cuenta) en la URL o en un parÃ¡metro, sin validar correctamente si el usuario autenticado tiene permisos para acceder a ese recurso.



**1ï¸â£ suario  legÃ­timo accede a su cuenta:**
https://app.universidad.com/account?id=1001


El sistema muestra la informaciÃ³n correspondiente a la cuenta **1001**.

---

**2ï¸ ManipulaciÃ³n  del parÃ¡metro por parte del atacante:**

El atacante modifica manualmente el valor del identificador en la URL:
https://app.universidad.com/account?id=1002


Si el sistema **no valida la autorizaciÃ³n correctamente**, mostrarÃ¡ la informaciÃ³n de la cuenta **1002**, que pertenece a otro usuario. Como resultado acceso no autorizado a informaciÃ³n de otra cuenta de usuario.

---

## Escalamiento Vertical de Privilegios

El **escalamiento vertical de privilegios** ocurre cuando un usuario con permisos bÃ¡sicos logra acceder a funcionalidades restringidas para administradores u otros roles con mayor nivel de acceso.


### Ejemplo de acceso indebido

Un usuario normal intenta acceder directamente a un recurso administrativo:
/admin/deleteUser


Si la aplicaciÃ³n no valida correctamente los permisos en el backend, el usuario podrÃ­a ejecutar acciones exclusivas de administrador.

---

### Bypass de autorizaciÃ³n mediante manipulaciÃ³n de JWT

En aplicaciones que utilizan **JSON Web Tokens (JWT)** para la autenticaciÃ³n, el atacante puede intentar modificar el contenido del token si este no estÃ¡ correctamente firmado o validado.

Ejemplo de campo manipulado:

```json
{
  "role": "admin"
}
```

Si el servidor no verifica correctamente la firma del token o confÃ­a Ãºnicamente en el contenido del campo role, el atacante podrÃ­a obtener privilegios de administrador.

### Las herramientas mÃ¡s usadas para explotar este tipo de vulnerabilidades son:
- Burp Suite
- OWASP ZAP
- Postman

## Casos reales 

- ExposiciÃ³n masiva de datos en APIs por falta de validaciÃ³n de permisos en endpoints REST.
- Un atacante simplemente fuerza a los navegadores a acceder a las URL objetivo. Se requieren derechos de administrador para acceder a la pÃ¡gina de administraciÃ³n.

## Mejores practicas 

- Implementar control de acceso en el backend
- Aplicar el principio de mÃ­nimo privilegio
- Validar permisos en cada request
- Usar RBAC o ABAC correctamente
- No confiar en datos del cliente (JWT sin validar)
- Realizar pruebas de autorizaciÃ³n automatizadas

---

## A02:2025 Security Misconfiguration
Ocurre cuando los sistemas, frameworks, servidores o aplicaciones estÃ¡n mal configurados generando vulnerabilidades

<p align="center">
  <img src="images/missconfiguration.png" width="600">
</p>

## Causas comunes

- Credenciales por defecto
- Puertos abiertos innecesarios
- Servicios expuestos
- Headers de seguridad ausentes
- Directory listing habilitado
- Errores detallados visibles

## Impacto

- Acceso no autorizado
- FiltraciÃ³n de informaciÃ³n
- Compromiso del servidor
- RCE (Remote Code Execution)

## MÃ©todos de ExplotaciÃ³n 

Uso de credenciales por defecto
Ejemplo:

admin / admin

## Acceso a paneles expuestos


/phpmyadmin
/admin

## EnumeraciÃ³n de directorios

https://site.university.com/uploads/


## Exploits conocidos en software sin parches
Herramientas usadas
- Nmap
- Nikto


## Casos reales
- ExposiciÃ³n pÃºblica de bases de datos Elasticsearch sin autenticaciÃ³n.
- Consolas administrativas expuestas en la nube.


## Mejores PrÃ¡cticas de PrevenciÃ³n
- Hardening de servidores
- Eliminar credenciales por defecto
- Aplicar parches regularmente
- Deshabilitar servicios innecesarios
- Implementar Infrastructure as Code segura

---

## A03:2025 Software Supply Chain Failures
Se refiere a vulnerabilidades introducidas a travÃ©s de dependencias externas, librerÃ­as, paquetes, contenedores o procesos CI/CD comprometidos. O cambios maliciosos en cÃ³digo, herramientas u otras dependencias de terceros de las que depende el sistema.

<p align="center">
  <img src="images/suministros.png" width="600">
</p>

## Naturaleza
- Uso de librerÃ­as vulnerables
- Dependencias maliciosas
- Ataques de dependency confusion
- Compromiso de repositorios

## Impacto
- EjecuciÃ³n remota de cÃ³digo
- Robo de credenciales
- Compromiso de pipelines
- DistribuciÃ³n de malware a clientes


## MÃ©todos de ExplotaciÃ³n

## Dependency Confusion

Publicar un paquete malicioso con el mismo nombre que uno interno.

## Compromiso de repositorios
Ejemplo:
Ataque a SolarWinds, Cisco, FORTINET

## InyecciÃ³n en pipeline CI/CD
- Modificar artefactos antes del despliegue.

 Herramientas utilizadas
- Snyk
- OWASP Dependency-Check
- npm audit

## Casos reales
- Actualizaciones comprometidas
- Dependencias vulnerables ampliamente utilizadas
- PublicaciÃ³n de paquetes maliciosos

---

## Mejores PrÃ¡cticas de PrevenciÃ³n
- Inventario de dependencias (SBOM)
- Escaneo continuo de vulnerabilidades
- VerificaciÃ³n de integridad (hashes)
- Firmado de artefactos
- Uso de repositorios privados
- RevisiÃ³n manual de dependencias crÃ­ticas
- Implementar DevSecOps en CI/CD










## A07:2021 - Identification and Authentication Failures





#Referencias 
https://owasp.org/www-project-top-ten/
https://www.indusface.com/learning/owasp-top-10-vulnerabilities/
https://owasp-org.translate.goog/Top10/2025/A01_2025-Broken_Access_Control/?_x_tr_sl=en&_x_tr_tl=es&_x_tr_hl=es&_x_tr_pto=tc
https://owasp-org.translate.goog/Top10/2025/A02_2025-Security_Misconfiguration/?_x_tr_sl=en&_x_tr_tl=es&_x_tr_hl=es&_x_tr_pto=tc
https://owasp-org.translate.goog/Top10/2025/A03_2025-Software_Supply_Chain_Failures/?_x_tr_sl=en&_x_tr_tl=es&_x_tr_hl=es&_x_tr_pto=tc
https://csrc.nist.gov/Projects/ssdf
https://www.enisa.europa.eu/publications/threat-landscape-for-supply-chain-attacks















