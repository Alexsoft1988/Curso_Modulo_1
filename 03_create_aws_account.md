# 🚀 Crear una cuenta AWS Free Tier – 2026

Guía paso a paso para crear una cuenta gratuita de **Amazon Web Services (AWS)** en 2026 y dejarla preparada para realizar laboratorios de Cloud, DevOps, Kubernetes, Terraform y otros servicios de AWS.

> ⚠️ **Importante:** AWS actualizó su programa Free Tier. Para cuentas nuevas, actualmente ofrece hasta **USD 200 en créditos** y un **Free account plan** que puede utilizarse durante un máximo de 6 meses o hasta consumir los créditos disponibles, lo que ocurra primero.

---

## 📋 Requisitos

Antes de comenzar, tener:

* 📧 Una dirección de correo electrónico.
* 📱 Un número de teléfono.
* 👤 Datos personales básicos.
* 🏠 Dirección de contacto.
* 💳 Un método de pago válido, si AWS lo solicita durante el registro.
* 🌎 País/región donde resides.

AWS utiliza estos datos para verificar la identidad y prevenir el abuso de las cuentas.

---

# 1. Crear la cuenta

Ingresar al sitio oficial:

[AWS Free Tier](https://aws.amazon.com/free/)

Seleccionar:

**Create a free account**

<img width="299" height="240" alt="image" src="https://github.com/user-attachments/assets/3f1fe63d-3c3d-4af8-874b-180605a12979" />

AWS permite actualmente utilizar credenciales existentes de proveedores como Google, GitHub, Apple o Amazon en determinadas experiencias nuevas de registro. La disponibilidad puede variar según la cuenta.

---

# 2. Crear las credenciales

Durante el registro se solicitará:

### Email

Ingresar una dirección de correo a la que tengas acceso.

Ejemplo:

```text
nombre.apellido@gmail.com
```

### AWS Account Name

Es el nombre que identificará la cuenta dentro de AWS.

Ejemplo:

```text
AWS-Lab-2026
```

> Este nombre se puede modificar posteriormente desde la consola de AWS.

<img width="346" height="484" alt="image" src="https://github.com/user-attachments/assets/58b51a71-2545-4b8f-be8d-489f33edb2a8" />

Dar click en Verify Email Adress

<img width="349" height="60" alt="image" src="https://github.com/user-attachments/assets/e53e619d-68cb-4bc7-b032-078a98e3d26c" />

---

# 3. Verificar cuenta

Abrir correo electrónico y verificar código

<img width="887" height="518" alt="image" src="https://github.com/user-attachments/assets/a9259f93-3bf9-492b-8ce1-ecadaa98988b" />

Introducir código en AWS y dar clic en verificar

<img width="447" height="439" alt="image" src="https://github.com/user-attachments/assets/b8725128-3c6d-42b8-bdc0-e5c3b1673bbb" />

# 4. Password

Colocar contraseña, luego dar clic en Continue

<img width="359" height="501" alt="image" src="https://github.com/user-attachments/assets/f503675c-cf3a-4b3b-84d6-a8cdef7ab658" />

# 5. Seleccionar el Free Account Plan

Durante el registro AWS puede mostrar dos opciones:

```text
Free account plan
Paid account plan
```

Para un laboratorio o entorno de aprendizaje:

```text
Free account plan
```
<img width="447" height="671" alt="image" src="https://github.com/user-attachments/assets/2dd6d626-963d-419d-abcb-8314135a7fa9" />

El **Free account plan** está diseñado para experimentar con AWS sin generar cargos durante el período gratuito. Finaliza después de 6 meses o cuando se consumen los créditos, lo que ocurra primero.

---

# 6. Completar información de contacto

Ingresar los datos solicitados:

```text
Full Name:
Country:
Address:
City:
State/Province:
Postal Code:
Phone Number:
```

Para Perú:

```text
Country: Peru
```

Utilizar información real para evitar problemas durante la verificación, luego dar clic en Agree and continue.

---

# 7. Método de pago

AWS puede solicitar un método de pago válido como parte del proceso de verificación.

Actualmente, AWS indica que el método de pago se utiliza para verificar la identidad y prevenir abusos. En el plan gratuito, AWS no debería realizar cargos mientras permanezcas en dicho plan; si se realiza una autorización temporal de la tarjeta para verificarla, puede aparecer una retención de aproximadamente USD 1 que posteriormente se libera.

<img width="770" height="611" alt="image" src="https://github.com/user-attachments/assets/5fa2ea3e-5aa9-492d-a441-dffa64e916b3" />


> ⚠️ **No asumir que "Free Tier" significa que cualquier servicio de AWS será gratuito.** Siempre revisar los límites y las condiciones de cada servicio.

---



# 8. Finalizar el registro

Completar los pasos restantes hasta que AWS confirme:

```text
Your AWS account is ready
```

Luego ingresar a:

[AWS Management Console](https://console.aws.amazon.com/)

---

# 9. Verificar que la cuenta esté activa

En la consola, verificar que puedas acceder a:

```text
AWS Management Console
```

y posteriormente a:

```text
Billing and Cost Management
```

También puedes revisar el estado de tu Free Tier y los créditos disponibles desde las herramientas de facturación de AWS.

---

# 🔐 10. Configurar MFA

Una de las primeras acciones recomendadas es habilitar MFA.

Ingresar a:

```text
IAM
└── Users
    └── Security credentials
```

o utilizar las opciones de seguridad disponibles para la cuenta raíz.

Configurar:

```text
MFA
```

Se recomienda utilizar una aplicación autenticadora compatible con TOTP.

> ⚠️ No utilizar la cuenta Root para las actividades habituales.

---

# 👤 11. Crear un usuario administrativo

Para trabajar normalmente, crear una identidad administrativa independiente de Root.

Ejemplo:

```text
usuario: aws-admin
```

Asignar los permisos administrativos necesarios para el laboratorio.

Posteriormente utilizar ese usuario para las tareas habituales.

---

# 💰 12. Configurar control de costos

Aunque estés utilizando el Free Tier, es recomendable controlar el consumo.

Ir a:

```text
Billing and Cost Management
```

y revisar:

```text
Free Tier
Credits
Bills
Budgets
Cost Explorer
```

AWS proporciona herramientas como **AWS Budgets** para establecer alertas y controlar el consumo.

---

# 📊 13. Crear un presupuesto

Crear un presupuesto para recibir alertas cuando el consumo alcance determinados valores.

Ejemplo conceptual:

```text
Budget:
AWS-Lab-2026

Monthly budget:
$5

Alert:
80%

Alert:
100%
```

Esto permite detectar rápidamente cualquier consumo inesperado.

> Nota: las alertas de presupuesto sirven para monitoreo; no deben considerarse un mecanismo que garantice que nunca habrá cargos.

---

# 🌎 14. Seleccionar una región

Para los laboratorios, seleccionar una región cercana a tu ubicación cuando sea apropiado.

Ejemplo:

```text
US East (N. Virginia)
us-east-1
```

o cualquier otra región soportada por el servicio que quieras utilizar.

Antes de crear recursos, verificar que el servicio y la oferta Free Tier estén disponibles en esa región.

---
