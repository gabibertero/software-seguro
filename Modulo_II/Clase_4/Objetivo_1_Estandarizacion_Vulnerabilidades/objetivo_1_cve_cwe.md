# Estandarización de Vulnerabilidades (CVE y CWE)

CVE y CWE no nombran lo mismo. Un **CVE** identifica una vulnerabilidad concreta que se hizo pública; un **CWE** clasifica el tipo de debilidad que la hizo posible. Por ejemplo, distintos productos pueden tener vulnerabilidades diferentes causadas por la misma clase de error. Esas fallas tendrían CVE distintos, pero podrían compartir un CWE.

Los CVE los asignan organizaciones autorizadas llamadas **CVE Numbering Authorities (CNAs)**. Cada una tiene un alcance definido, como los productos de una empresa o de un proyecto. Después puedo consultar esos registros en CVE.org o en la NVD, que agrega información complementaria.

Dos herramientas que pueden ayudar a detectar vulnerabilidades conocidas son **OWASP ZAP**, para analizar aplicaciones web, y **Nmap**, que tiene scripts NSE para comprobar ciertas fallas. Ninguna detecta todo ni reemplaza la revisión manual.
