# 🚀 Crear una instancia EC2 para Jenkins – AWS 2026

Guía para crear una instancia **Amazon EC2** que será utilizada como servidor de **Jenkins** para un laboratorio de DevOps.

> ⚠️ **Importante:** esta guía está pensada para un entorno de laboratorio. El Security Group se configurará con tráfico entrante abierto desde Internet (`0.0.0.0/0`), tal como se solicita para este laboratorio. **No utilices esta configuración en producción.**

---

## 📋 Requisitos

Antes de comenzar necesitas:

- Una cuenta AWS activa.
- Acceso a la **AWS Management Console**.
- Un navegador web.
- Una región AWS seleccionada.
- Un Key Pair para conectarte por SSH.

---

# 1. Ingresar a Amazon EC2

Desde la consola de AWS ingresar a:

**EC2 → Instances**

Seleccionar:

**Launch instance**

---

# 2. Configurar el nombre de la instancia

En **Name and tags**, colocar:

```text
jenkins-server
```

También puedes utilizar:

```text
jenkins-lab
aws-jenkins
jenkins-devops
```

Para este laboratorio utilizaremos:

```text
jenkins-server
```

---

# 3. Seleccionar la AMI

En **Application and OS Images (Amazon Machine Image)** seleccionar:

**Quick Start → Amazon Linux**

Luego seleccionar:

**Amazon Linux 2023 AMI**

Configuración recomendada:

```text
Operating System:
Amazon Linux 2023

Architecture:
64-bit (x86)
```
<img width="947" height="541" alt="image" src="https://github.com/user-attachments/assets/ca59cb85-a837-42bf-af30-2a9fb772a285" />

La AMI debe mostrar la etiqueta:

```text
Free tier eligible
```

si la oferta gratuita de tu cuenta aplica a esa configuración.

> 💡 Para este laboratorio utilizaremos Amazon Linux 2023 porque es una distribución mantenida por AWS y resulta adecuada para instalar Jenkins y otras herramientas de DevOps.

---

# 4. Seleccionar el tipo de instancia

En **Instance type** seleccionar una instancia adecuada para Jenkins.

Para un laboratorio básico:

```text
t3.small
```

Para un Jenkins que ejecutará varios builds:

```text
t3.medium
```

### Recomendación para este laboratorio

Si únicamente vas a instalar Jenkins y realizar algunos pipelines:

```text
t3.small
```

Si vas a ejecutar builds de:

- Java / Maven
- Gradle
- Node.js
- Docker
- Terraform
- Trivy
- SonarScanner

es preferible utilizar:

```text
m7i-flex.large
```
<img width="998" height="359" alt="image" src="https://github.com/user-attachments/assets/b3f395c6-4287-40e4-a202-acad446adad9" />

> ⚠️ Revisa siempre la etiqueta **Free tier eligible** y el costo estimado antes de lanzar la instancia. El hecho de que una cuenta tenga créditos o beneficios gratuitos no significa que cualquier tipo de instancia sea gratuito.

---

# 5. Crear el Key Pair

En **Key pair (login)** seleccionar:

**Create new key pair**

Configurar:

```text
Name:
jenkins-lab-key

Key pair type:
RSA

Private key file format:
.pem
```

Seleccionar:

**Create key pair**

El navegador descargará:

```text
jenkins-lab-key.pem
```

### ⚠️ Importante

Guarda este archivo en un lugar seguro.

En macOS/Linux puedes ejecutar:

```bash
chmod 400 jenkins-lab-key.pem
```

No compartas el archivo `.pem`.

---

# 6. Configurar Network settings

Desplegar:

**Network settings**

Configurar:

```text
VPC:
Default VPC

Subnet:
No preference

Auto-assign public IP:
Enable
```

La opción:

```text
Auto-assign public IP: Enable
```

es importante porque necesitamos acceder a Jenkins desde Internet.

---

# 7. Crear el Security Group

En:

**Firewall (security groups)**

Seleccionar:

```text
Create security group
```

Configurar:

```text
Security group name:
jenkins-lab-sg

Description:
Security group for Jenkins laboratory
```
<img width="934" height="630" alt="image" src="https://github.com/user-attachments/assets/45d48d56-fbfd-4a2f-ac54-6c585ca5e856" />

---

# 8. Abrir todo el tráfico

Para este laboratorio queremos permitir el acceso desde cualquier dirección IPv4.

Configurar una regla:

```text
Type:
All traffic

Protocol:
All

Port range:
All

Source:
Anywhere-IPv4

CIDR:
0.0.0.0/0
```

El resultado será:

```text
Inbound
    ↓
All traffic
    ↓
0.0.0.0/0
```

<img width="957" height="538" alt="image" src="https://github.com/user-attachments/assets/0b805d64-7e9b-4f9a-b189-6bbef7dfd75c" />

Esto permitirá acceder a la instancia desde Internet.

### ⚠️ Advertencia de seguridad

La configuración:

```text
0.0.0.0/0
```

permite conexiones desde cualquier dirección IPv4.

Esto significa que la instancia estará expuesta directamente a Internet.

**Esta configuración es únicamente para el laboratorio.**

En un entorno real se recomienda restringir el acceso, por ejemplo:

```text
SSH     TCP 22     → Tu IP
HTTP    TCP 80     → Según necesidad
HTTPS   TCP 443    → Según necesidad
Jenkins TCP 8080   → Según necesidad
```

Para este laboratorio mantendremos:

```text
All traffic → 0.0.0.0/0
```

---

# 9. Configurar el almacenamiento

En **Configure storage** utilizar:

```text
1 x 50 GiB
Volume type:
gp3
```

Configuración:

```text
Size:
50 GiB

Volume type:
gp3

Root volume:
Yes
```

### ¿Por qué 50 GiB?

Jenkins utiliza almacenamiento para:

```text
Jenkins Home
├── Jobs
├── Builds
├── Plugins
├── Workspaces
├── Logs
└── Artifacts
```

Además, durante los pipelines es posible utilizar herramientas y dependencias como:

```text
Git
Maven
Gradle
Node.js
Docker
Terraform
Trivy
SonarScanner
```

Por eso, para este laboratorio utilizaremos:

```text
50 GiB gp3
```

Es un volumen de propósito general adecuado para comenzar con Jenkins.

> 💡 Si posteriormente necesitas más espacio, puedes aumentar el tamaño del volumen EBS.

---

# 10. File systems

En **File systems** seleccionar:

```text
None
```

No necesitamos:

```text
S3 Files
EFS
FSx
```

para esta instalación inicial de Jenkins.

---

# 11. Revisar la configuración

Antes de lanzar la instancia, el **Summary** debería ser similar a:

```text
Name:
jenkins-server

AMI:
Amazon Linux 2023

Architecture:
64-bit (x86)

Instance type:
t3.small

Key pair:
jenkins-lab-key

Public IP:
Enabled

Security group:
jenkins-lab-sg

Inbound:
All traffic
0.0.0.0/0

Storage:
50 GiB
gp3

File system:
None
```

---

# 12. Lanzar la instancia

En el panel **Summary** seleccionar:

**Launch instance**

AWS mostrará la confirmación del lanzamiento.

La instancia inicialmente aparecerá como:

```text
Pending
```

Después de unos segundos debería cambiar a:

```text
Running
```

---

# 13. Obtener la IP pública

Ir a:

**EC2 → Instances**

Seleccionar:

```text
jenkins-server
```

Buscar:

```text
Public IPv4 address
```

Ejemplo:

```text
18.XXX.XXX.XXX
```

También puedes utilizar:

```text
Public IPv4 DNS
```

---

# 14. Conectarse por SSH

En macOS o Linux:

```bash
chmod 400 jenkins-lab-key.pem
```

Luego:

```bash
ssh -i jenkins-lab-key.pem ec2-user@<PUBLIC_IP>
```

Ejemplo:

```bash
ssh -i jenkins-lab-key.pem ec2-user@18.XXX.XXX.XXX
```

Para Amazon Linux 2023 el usuario utilizado para la conexión SSH es:

```text
ec2-user
```

---

# 15. Verificar la instancia

Una vez conectado:

```bash
uname -a
```

Verificar la versión del sistema:

```bash
cat /etc/os-release
```

Verificar el almacenamiento:

```bash
df -h
```

También puedes revisar los discos:

```bash
lsblk
```

Deberías observar un volumen raíz cercano a:

```text
50G
```

<img width="905" height="391" alt="image" src="https://github.com/user-attachments/assets/43ed2b23-81e1-48be-8b87-b8f54eaef430" />

---

# 16. Actualizar Amazon Linux

Antes de instalar Jenkins:

```bash
sudo dnf update -y
```

Una vez finalizada la actualización:

```bash
sudo reboot
```

Espera unos segundos y vuelve a conectarte por SSH.

---

# 17. Preparar Java para Jenkins

Jenkins necesita Java para ejecutarse.

Antes de instalar Jenkins, verifica la versión disponible:

```bash
java -version
```

Si Java todavía no está instalado, instala la versión compatible con la versión de Jenkins que utilizarás.

> ⚠️ La versión de Java soportada depende de la versión de Jenkins. Antes de instalarlo, revisa la documentación oficial de la versión de Jenkins que utilizarás.

---

# 18. Acceder a Jenkins

La instalación estándar de Jenkins utiliza el puerto:

```text
8080
```

Una vez instalado y ejecutándose Jenkins, podrás acceder desde tu navegador:

```text
http://<PUBLIC_IP>:8080
```

Ejemplo:

```text
http://18.XXX.XXX.XXX:8080
```

Debido a que el Security Group de este laboratorio permite:

```text
All traffic
0.0.0.0/0
```

el puerto `8080` podrá recibir conexiones desde Internet.

---

# 19. Arquitectura del laboratorio

La configuración final será:

```text
                         INTERNET
                            │
                            │
                       0.0.0.0/0
                            │
                            ▼
                 ┌────────────────────┐
                 │   Security Group   │
                 │    jenkins-lab-sg  │
                 │                    │
                 │    All traffic     │
                 │    0.0.0.0/0       │
                 └──────────┬─────────┘
                            │
                            ▼
                 ┌────────────────────┐
                 │        EC2         │
                 │                    │
                 │ Amazon Linux 2023  │
                 │                    │
                 │      Jenkins       │
                 │       :8080        │
                 └──────────┬─────────┘
                            │
                            ▼
                 ┌────────────────────┐
                 │      EBS gp3       │
                 │       30 GiB       │
                 │                    │
                 │ Jenkins Home       │
                 │ Workspaces         │
                 │ Builds             │
                 │ Plugins            │
                 │ Artifacts          │
                 └────────────────────┘
```

---

# 20. Checklist

Antes de seleccionar **Launch instance**:

- [ ] Nombre: `jenkins-server`
- [ ] Amazon Linux 2023
- [ ] Arquitectura: `64-bit (x86)`
- [ ] Instance type seleccionado
- [ ] Revisar `Free tier eligible`
- [ ] Key Pair creado: `jenkins-lab-key`
- [ ] Public IP: `Enable`
- [ ] Security Group: `jenkins-lab-sg`
- [ ] Inbound: `All traffic`
- [ ] Source: `0.0.0.0/0`
- [ ] Storage: `30 GiB`
- [ ] Volume type: `gp3`
- [ ] File systems: `None`
- [ ] Revisar Summary
- [ ] Launch instance

---

# ⚠️ Control de costos

Antes de lanzar la instancia revisa:

**Billing and Cost Management**

y verifica los créditos y/o beneficios disponibles en tu cuenta.

Ten en cuenta:

- Una instancia EC2 en ejecución puede generar costos.
- El almacenamiento EBS puede generar costos.
- Otros recursos asociados también pueden generar costos.
- Los créditos disponibles no necesariamente cubren todos los servicios.

Cuando termines el laboratorio, elimina los recursos que ya no necesites.

---

# 21. Eliminar la instancia

Cuando ya no necesites Jenkins:

**EC2 → Instances → jenkins-server → Terminate instance**

No confundas:

```text
Stop
```

con:

```text
Terminate
```

**Stop** detiene la instancia, pero conserva los recursos asociados.

**Terminate** elimina la instancia.

Antes de terminarla, verifica también los recursos asociados, como:

```text
EBS volumes
Elastic IPs
Load Balancers
NAT Gateways
```

---

# 📚 Documentación oficial

- [Amazon EC2](https://aws.amazon.com/ec2/)
- [Launch an EC2 instance](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/LaunchingAndUsingInstances.html)
- [EC2 Security Groups](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-security-groups.html)
- [Amazon Linux 2023](https://docs.aws.amazon.com/linux/al2023/ug/ec2.html)
- [Amazon EBS](https://docs.aws.amazon.com/ebs/)

---

## 🎯 Objetivo

Al finalizar tendrás:

```text
AWS
 │
 ▼
EC2
 │
 ├── Amazon Linux 2023
 │
 ├── Jenkins
 │      └── :8080
 │
 ├── Security Group
 │      └── All traffic
 │          └── 0.0.0.0/0
 │
 └── EBS
        └── gp3
            └── 30 GiB
```

Este servidor será la base para los siguientes laboratorios de:

```text
Git
 ↓
Jenkins
 ↓
CI/CD
 ↓
Docker
 ↓
Terraform
 ↓
AWS
 ↓
Kubernetes
```
