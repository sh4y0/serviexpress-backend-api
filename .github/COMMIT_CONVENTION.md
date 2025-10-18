# 🧩 Convenciones de Commits — Serviexpress Backend

> Basado en [Conventional Commits v1.0.0](https://www.conventionalcommits.org/en/v1.0.0/)  
> Adaptado para monorepo con múltiples microservicios (Spring WebFlux, Java 21)

---

## 🎯 Propósito

Estas convenciones aseguran **claridad, trazabilidad y automatización** entre desarrolladores y pipelines CI/CD.

Cada commit debe reflejar **qué cambió**, **dónde cambió** (microservicio o módulo) y **por qué**.

### Estructura General

```
<tipo>(<área>): <descripción corta>
```

---

## 🔖 Tipos de Commit

| Tipo | Emoji | Descripción | Ejemplo |
|------|:-----:|-------------|---------|
| **feat** | ✨ | Nueva funcionalidad o endpoint | `feat(auth-service): agrega endpoint de login reactivo` |
| **fix** | 🐛 | Corrección de errores | `fix(loan-service): corrige validación de monto` |
| **refactor** | ♻️ | Cambio interno sin afectar funcionalidad | `refactor(shared): simplifica conversión de DTOs` |
| **perf** | ⚡ | Mejora de rendimiento | `perf(auth-service): optimiza consulta de usuarios` |
| **style** | 💎 | Cambios de formato o estilo | `style(loan-service): aplica format con Spotless` |
| **test** | 🧪 | Nuevas pruebas o modificaciones | `test(auth-service): agrega tests para JWT` |
| **docs** | 📝 | Actualización de documentación | `docs: actualiza guía de instalación` |
| **chore** | 🔧 | Tareas menores (config, dependencias) | `chore(deps): actualiza Spring Boot a 3.5.4` |
| **build** | 📦 | Cambios en build o dependencias | `build(gradle): configura plugin de Docker` |
| **ci** | 👷 | Cambios en configuración CI/CD | `ci(github): optimiza pipeline de tests` |
| **revert** | ⏪ | Reversión de commit previo | `revert: revierte "feat(auth): agrega 2FA"` |

---

## 📁 Contexto de Módulo / Servicio

En un **monorepo**, especifica el microservicio o módulo afectado entre paréntesis.

### Ejemplos por Área

```bash
# Microservicios
feat(auth-service): agrega endpoint de login reactivo
fix(loan-service): corrige validación de monto negativo
perf(notification-service): optimiza envío masivo de emails

# Módulos compartidos
refactor(shared): mueve clase DTO común a shared-lib
test(shared-utils): agrega tests unitarios para validadores

# Configuración global
build(root): actualiza versión de Spring Boot a 3.5.4
ci(github-actions): optimiza ejecución de test matrix

# Documentación
docs(readme): actualiza instrucciones de despliegue
```

---

## 🧱 Estructura Completa del Commit

```
<tipo>(<área>): <descripción corta>

[cuerpo opcional]

[footer opcional]
```

### ✏️ Descripción Corta

- ✅ Máximo **100 caracteres**
- ✅ Modo imperativo: *agrega*, *corrige*, *actualiza*
- ✅ Primera letra en minúscula
- ✅ Sin punto final

### 🧩 Cuerpo (Opcional)

Describe el **por qué** del cambio, el contexto o implicaciones técnicas.

```
feat(auth-service): agrega soporte JWT en login

Ahora el login genera tokens firmados con RS256.
Se agregó dependencia spring-security-oauth2-jose.
Los tokens tienen TTL de 1 hora configurable vía properties.
```

### 🧾 Footer (Opcional)

Usado para:
- 🔗 Referencias a tickets: `Closes #123`, `Refs #456`
- ⚠️ Breaking changes: `BREAKING CHANGE: descripción`

```
feat(loan-service): cambia estructura de respuesta API

BREAKING CHANGE: el endpoint /loans ahora retorna array en lugar de objeto
Closes #45
```

---

## 🎨 Ejemplos Válidos

### ✅ Correctos

```bash
feat(loan-service): implementa endpoint para listar préstamos activos

fix(auth-service): corrige error de validación en login

refactor(shared): simplifica conversión de DTOs

perf(notification-service): reduce tiempo de envío de emails en 40%

test(loan-service): agrega tests de integración para cálculo de intereses

chore(ci): mejora velocidad del pipeline con caché de Gradle

docs: agrega guía para configuración local de Gradle

build(dependencies): actualiza reactor-core a 3.6.0
```

### ❌ Incorrectos

```bash
❌ update code                    # Muy genérico, sin tipo ni área
❌ bug fix                        # Sin área específica
❌ added stuff                    # Descripción vaga
❌ login fixed                    # Sin tipo formal
❌ Fix: corrección login.         # Tipo en mayúscula y con punto final
❌ feat(auth): Login agregado     # Descripción en pasado
```

---

## 🎯 Versionado Semántico (SemVer)

Los commits afectan el versionado automático según su tipo:

| Tipo de Cambio | Ejemplo de Commit | Versión Afectada |
|----------------|-------------------|------------------|
| 🐛 **Patch** | `fix(auth): corrige error de login` | `1.0.0` → `1.0.1` |
| ✨ **Minor** | `feat(loan): nuevo endpoint de solicitud` | `1.0.0` → `1.1.0` |
| 💥 **Major** | `feat!: cambia estructura de respuesta` | `1.0.0` → `2.0.0` |

### Breaking Changes

Para indicar cambios que rompen compatibilidad:

```bash
# Opción 1: Usar ! después del tipo/área
feat(api)!: cambia formato de respuesta JSON

# Opción 2: Agregar BREAKING CHANGE en footer
feat(shared): renombra DTOs principales

BREAKING CHANGE: PersonDTO ahora se llama UserDTO
```

Esto permite automatizar **changelogs** y **releases** con herramientas como:
- `semantic-release`
- `release-please`
- `conventional-changelog`

---

## 🌿 Convención para Ramas

| Tipo de Rama | Patrón | Ejemplo |
|--------------|--------|---------|
| 🌟 **Feature** | `feature/<nombre>` | `feature/login-reactivo` |
| 🐛 **Fix** | `fix/<nombre>` | `fix/validacion-loan` |
| ♻️ **Refactor** | `refactor/<nombre>` | `refactor/shared-utils` |
| 🔥 **Hotfix** | `hotfix/<nombre>` | `hotfix/cors-prod` |
| 📚 **Docs** | `docs/<nombre>` | `docs/api-swagger` |
| 🧪 **Test** | `test/<nombre>` | `test/integration-auth` |

### Flujo de Trabajo

```
main (producción)
  ↑
develop (integración)
  ↑
feature/nueva-funcionalidad → PR → merge
```

---

## 🚀 Relación con CI/CD

### Automatización del Pipeline

- ✅ **CI** se ejecuta en cada `push` o `PR` hacia `develop` o `main`
- ✅ **CD** se dispara al hacer `merge` a `main` o crear tag `vX.Y.Z`
- 🏷️ **Tags automáticos** basados en conventional commits

### Beneficios

| Acción | Resultado |
|--------|-----------|
| 🤖 **Changelogs** | Generación automática de CHANGELOG.md |
| 📦 **Versionado** | Incremento automático de versión (SemVer) |
| 🎯 **Trazabilidad** | Identificación de servicios afectados en monorepo |
| 🔍 **Auditoría** | Historial claro de cambios por tipo y área |

---

## ✅ Checklist Antes de Hacer Commit

Antes de ejecutar `git commit`, verifica:

- [ ] ✅ Usas un tipo válido (`feat`, `fix`, `refactor`, etc.)
- [ ] ✅ Incluyes el módulo o servicio afectado `(área)`
- [ ] ✅ La descripción es clara y concisa (≤100 chars)
- [ ] ✅ Usas modo imperativo (*agrega*, no *agregado*)
- [ ] ✅ Si rompe compatibilidad, agregas `BREAKING CHANGE:`
- [ ] ✅ Ejecutaste `./gradlew clean build test` localmente
- [ ] ✅ El código cumple con las reglas de estilo (Spotless/Checkstyle)
- [ ] ✅ Agregaste tests si es nueva funcionalidad

---

## 💡 Ejemplos Completos Detallados

### Ejemplo 1: Feature con Breaking Change

```
feat(auth-service): agrega validación JWT y endpoint refresh-token

Se agrega soporte para renovación de tokens JWT usando
la librería spring-security-oauth2-jose. Los tokens ahora
incluyen claims personalizados (roles, permissions).

Configuración:
- TTL access token: 1h (configurable)
- TTL refresh token: 7d (configurable)

BREAKING CHANGE: el endpoint /login ahora retorna 401 en lugar
de 403 para credenciales inválidas

Closes #102
```

### Ejemplo 2: Fix con Contexto Técnico

```
fix(loan-service): corrige cálculo de interés en pagos parciales

El cálculo de interés compuesto fallaba para préstamos
con pagos fuera de calendario. Se ajustó la fórmula para
considerar días transcurridos reales.

Antes: rate * principal
Ahora: rate * principal * (days/365)

Refs #87
```

### Ejemplo 3: Refactor Importante

```
refactor(shared): migra DTOs a records de Java 21

Se reemplazaron 15 clases DTO tradicionales por records.
Beneficios:
- Código más conciso (-200 LOC)
- Inmutabilidad por defecto
- Mejor performance en serialización

BREAKING CHANGE: constructores de DTOs ahora son compactos
Los clientes deben actualizar instanciación de objetos

Closes #156
```

### Ejemplo 4: Optimización de Performance

```
perf(notification-service): implementa envío batch de emails

Se agrupa el envío de notificaciones en lotes de 50.
Mejoras medidas:
- Throughput: 100 → 500 emails/min
- Latencia p95: 2s → 400ms
- Uso de conexiones SMTP: -80%

Refs #203
```

### Ejemplo 5: CI/CD Optimization

```
ci(github-actions): optimiza cacheo de dependencias Gradle

Se implementa caché multinivel para:
- Gradle wrapper
- Dependencies (.gradle/caches)
- Build outputs (build/)

Resultado: tiempo de build reducido de 8min a 2min

Closes #178
```

---

<div align="center">

**© 2025 Serviexpress Backend Team**

*Construyendo software de calidad, un commit a la vez* 🚀

</div>