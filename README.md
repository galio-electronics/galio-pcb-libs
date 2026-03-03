# Altium Libraries Repository

Repositorio maestro para la administración de librerías de **Altium Designer** usando únicamente archivos:

* `.SchLib` para símbolos esquemáticos
* `.PcbLib` para footprints de PCB

El objetivo de este repositorio es mantener una base de componentes **simple, ordenada, trazable y colaborativa**, utilizando **Git** como sistema de control de versiones.

---

## Objetivo

Este repositorio define una forma de trabajo estandarizada para:

* Centralizar las librerías de componentes
* Evitar duplicados y versiones dispersas
* Mantener trazabilidad de cambios
* Facilitar colaboración entre desarrolladores
* Reducir ruido en el repositorio excluyendo archivos temporales y generados automáticamente por Altium

La **fuente de verdad** de las librerías es este repositorio.

---

## Alcance

Este repositorio almacena únicamente archivos de librería editables y documentación asociada.

### Archivos permitidos

* `.SchLib`
* `.PcbLib`
* `.md`
* scripts o utilidades de soporte, cuando aplique

### Archivos no considerados fuente de verdad

* `.IntLib`
* archivos de salida de fabricación
* archivos temporales
* respaldos automáticos
* historial local de Altium
* carpetas de outputs generados

---

## Estructura del repositorio

Se recomienda organizar las librerías por **tipo de componente** y **familia funcional**.

```text
galio-pcb-libs/
│
├─ .gitignore
├─ README.md
├─ CHANGELOG.md
├─ docs/
│  ├─ conventions.md
│  ├─ review-checklist.md
│  └─ workflow.md
│
├─ libraries/
│  ├─ passives/
│  │  ├─ resistors/
│  │  │  ├─ resistors.SchLib
│  │  │  ├─ resistors.PcbLib
│  │  │  └─ README.md
│  │  ├─ capacitors/
│  │  │  ├─ capacitors.SchLib
│  │  │  ├─ capacitors.PcbLib
│  │  │  └─ README.md
│  │
│  ├─ connectors/
│  │  ├─ usb/
│  │  │  ├─ usb.SchLib
│  │  │  ├─ usb.PcbLib
│  │  │  └─ README.md
│  │  ├─ terminal_blocks/
│  │
│  ├─ ics/
│  │  ├─ mcu/
│  │  ├─ power/
│  │  ├─ analog/
│  │  └─ interfaces/
│  │
│  ├─ electromechanical/
│  │  ├─ relays/
│  │  ├─ switches/
│  │  └─ displays/
│  │
│  └─ modules/
│     ├─ esp32/
│     ├─ lora/
│     └─ sensors/
│
└─ archive/
   └─ deprecated/
```

---

## Principios de administración

### 1. Simplicidad

Se trabaja únicamente con `.SchLib` y `.PcbLib` como archivos principales.

### 2. Organización por familias

Cada carpeta representa una familia lógica de componentes.

### 3. Trazabilidad

Todo cambio debe quedar registrado en Git mediante commits claros y revisables.

### 4. Consistencia

Los nombres, descripciones y estructura deben seguir convenciones comunes.

### 5. Estabilidad

La rama principal solo debe contener librerías revisadas y aprobadas.

---

## Convenciones de nombres

### Archivos de librería

Usar nombres simples, estables y descriptivos:

* `resistors.SchLib`
* `resistors.PcbLib`
* `usb.SchLib`
* `usb.PcbLib`
* `stm32_mcu.SchLib`
* `stm32_mcu.PcbLib`

Evitar nombres como:

* `Library_Final_v3.SchLib`
* `NuevaLibreria_okahora_si.PcbLib`
* `componentes_actualizados2.SchLib`

Sí, Git recuerda todo; no hace falta ponerle ansiedad al nombre.

### Carpetas

Usar nombres en minúsculas, con `_` si es necesario:

* `terminal_blocks`
* `power_management`
* `ble_modules`

### Componentes dentro de las librerías

Se recomienda seguir una convención consistente que permita identificar rápidamente:

* fabricante
* número de parte
* encapsulado o footprint
* variante funcional

Ejemplo:

* `STM32F103C8T6_LQFP48`
* `AMS1117_3V3_SOT223`
* `USB_C_16P_MIDMOUNT`
* `R_0603`
* `C_0805`

---

## Contenido mínimo por familia

Cada carpeta de familia debe contener, idealmente:

* un archivo `.SchLib`
* un archivo `.PcbLib`
* un `README.md` opcional con notas específicas

### Ejemplo de README por familia

Puede incluir:

* criterio de nomenclatura
* datasheets de referencia
* notas de uso
* limitaciones conocidas
* historial importante de cambios

---

## Política de ramas

### Rama principal

* `main` = versión estable y aprobada de las librerías

### Ramas de trabajo

Toda modificación debe hacerse en una rama separada:

* `feat/nueva-familia-usb`
* `feat/stm32-symbols`
* `fix/footprint-rj45`
* `refactor/passives-naming`

### Reglas

* No hacer push directo a `main`
* Toda integración debe realizarse mediante Pull Request
* Una rama debe enfocarse en un cambio concreto
* No mezclar correcciones, refactors y componentes nuevos en la misma rama si no están relacionados

---

## Flujo de trabajo recomendado

### 1. Actualizar rama principal

```bash
git checkout main
git pull origin main
```

### 2. Crear rama nueva

```bash
git checkout -b feat/usb-connectors
```

### 3. Realizar cambios

Agregar o corregir símbolos y footprints dentro de la familia correspondiente.

### 4. Revisar cambios

Validar antes de hacer commit:

* nombres correctos
* pines correctos
* numeración de pads correcta
* correspondencia contra datasheet
* polaridad clara
* silkscreen y courtyard razonables
* descripción legible
* que no se hayan agregado archivos temporales

### 5. Commit

```bash
git add .
git commit -m "feat(usb): add USB-C connector symbols and footprints"
```

### 6. Push

```bash
git push origin feat/usb-connectors
```

### 7. Pull Request

Abrir PR hacia `main` con:

* resumen del cambio
* componentes agregados o corregidos
* referencia al datasheet
* notas de validación

### 8. Revisión y merge

Solo después de revisión técnica se aprueba el merge a `main`.

---

## Reglas de colaboración

### Regla 1: evitar edición simultánea del mismo archivo

Los archivos `.SchLib` y `.PcbLib` no son ideales para merges complejos. Por ello:

* evitar que dos personas editen la misma librería al mismo tiempo
* coordinar responsables por familia de componentes
* dividir el trabajo por carpetas o bloques funcionales

### Regla 2: cambios pequeños y específicos

Cada PR debe ser lo más acotado posible.

### Regla 3: respaldar cambios con datasheet

Todo componente nuevo o modificado debe estar basado en documentación técnica oficial.

### Regla 4: no subir basura operativa

No deben subirse al repositorio:

* `History`
* `__Previews`
* outputs de fabricación
* logs
* backups
* temporales
* exports automáticos

---

## Checklist mínimo de revisión

Antes de aprobar un cambio, revisar:

### Símbolo esquemático

* nombres de pines correctos
* tipos eléctricos coherentes
* número de pin correcto
* símbolo claro y legible
* designator y comment bien definidos

### Footprint PCB

* dimensiones correctas según datasheet
* pads correctos
* pin 1 claramente marcado
* silkscreen útil
* courtyard razonable
* origen bien colocado
* nombre de footprint consistente

### Integridad general

* nombre correcto del componente
* relación símbolo-footprint coherente
* familia correcta
* sin archivos basura añadidos al commit

---

## Buenas prácticas recomendadas

* Mantener una librería por familia, no una sola mega biblioteca universal
* Documentar excepciones o decisiones especiales
* Hacer commits pequeños y descriptivos
* Revisar diffs antes de hacer push
* Usar `archive/deprecated/` para retirar contenido obsoleto sin borrarlo abruptamente
* Crear tags de versión cuando exista un lote estable importante

---

## Versionado recomendado

Se pueden manejar tags estables por fecha o por release interno:

* `libs-2026.03`
* `libs-2026.03-r1`
* `libs-2026-q1`

Esto permite congelar una versión de librerías para proyectos productivos y mantener trazabilidad.

---

## Filosofía del repositorio

Este repositorio busca priorizar:

* orden
* claridad
* colaboración
* estabilidad
* mantenimiento a largo plazo

La meta no es solo guardar archivos, sino construir una **base reutilizable de componentes confiables** para diseño electrónico.

En otras palabras: menos “¿quién movió mi footprint?” y más ingeniería con flujo.

---

## Responsable del repositorio

Definir responsables por familia o por disciplina, por ejemplo:

* administrador general del repositorio
* responsable de símbolos
* responsable de footprints
* revisores técnicos

Esto ayuda a evitar cambios sin dueño y decisiones de diseño tomadas al estilo “feeling-driven development”.

---

## Notas finales

Este repositorio debe considerarse la referencia oficial para el equipo de diseño electrónico.

Cualquier librería local, temporal o experimental debe validarse antes de incorporarse a `main`.

Si un componente no está aquí, para efectos del proceso, **todavía no existe oficialmente**.

---
