# Documentación Técnica - Copybook PDBSI1.txt

## Análisis y Descomposición de la Copybook

### Información General
- **Programa:** PDBS1SI  
- **Autor:** JRC
- **Fecha Creación:** 3 de abril, 1997
- **Propósito:** Acceso y mantenimiento de los datos generales de UN SINIESTRO
- **Ubicación:** /EXPL/APLICACIONES-AUTOS-PRESTACIONES_CICSD/PDBS1SI.CICSD

## 1. División de Registros en Niveles Lógicos

### Nivel 01 - Estructuras Principales

#### 1.1 WS-ERR-SIDC (Línea 599)
```cobol
01  WS-ERR-SIDC                     PIC X(37).
```
**Función de Negocio:** Almacena mensajes de error del módulo SIDC (Sistema de Control de Siniestros).
**Uso en el Proceso:** Control de errores y manejo de excepciones en operaciones de consulta y actualización de siniestros.

#### 1.2 WS-PDBSCCM (Línea 601)
```cobol
01  WS-PDBSCCM.
    05 WS-GARANTIA-3                PIC X(1).
    05 WS-FCURFEAC-SCCM             PIC X(10).
    05 WS-FCURCULP-SCCM             PIC 9(1).
    05 WS-FCURTSIN-SCCM             PIC 9(2).
    05 WS-FCURMATR-SCCM             PIC X(12).
    05 WS-FCURSINI-SCCM             PIC 9(11).
    05 WS-FCURFRAN-SCCM             PIC X(1).
    05 WS-FCURFRA2-SCCM             PIC X(1).
    05 WS-FCURTIMP-SCCM             PIC 9(1).
    05 WS-FCURDANO-SCCM             PIC 9(2).
    05 WS-FCURPOLI-SCCM             PIC 9(9).
    05 WS-FCURSITU-SCCM             PIC X(1).
    05 WS-FCURPOCU-SCCM             PIC 9(2).
    05 WS-FCURCCIA-SCCM             PIC 9(3).
    05 WS-FCURCONV-SCCM             PIC 9(1).
```
**Función de Negocio:** Estructura de datos para comunicación con el módulo PDBSCCM (Sistema de Contrarios y Clientes).
**Uso en el Proceso:** Intercambio de información de siniestros entre módulos del sistema.

#### 1.3 WS-VARIABLES (Línea 618)
```cobol
01  WS-VARIABLES.
    05 WS-FCUROPER                  PIC X(04).
    05 WS-FCURCANA                  PIC X(01).
    05 WS-FCURECMA                  PIC X(50).
    05 WS-FRENT                     PIC X(10).
```
**Función de Negocio:** Variables de trabajo para operaciones generales del programa.
**Uso en el Proceso:** Almacenamiento temporal de datos durante el procesamiento de siniestros.

#### 1.4 Estructura de Fechas WS-FEAC-10 (Línea 625)
```cobol
05 WS-FEAC-10.
   10 WS-DIA-10                 PIC 9(2).
   10 WS-P1                     PIC X(01).
   10 WS-MES-10                 PIC 9(2).
   10 WS-P2                     PIC X(01).
   10 WS-ANIO-10                PIC 9(4).
```
**Función de Negocio:** Formato de fecha extendido para presentación en pantalla (DD.MM.AAAA).
**Uso en el Proceso:** Conversión y formateo de fechas para interfaz de usuario.

#### 1.6 WS-SWITCHES (Línea 1243)
```cobol
01  WS-SWITCHES.
    05 SW-VISIONADO-SIDC           PIC X(1).
       88 NO-CONSULTAR                         VALUE 'N'.
       88 SI-PDTE-VER-SIDC                     VALUE 'P'.
       88 SI-VISTO-SIDC                        VALUE 'V'.
       88 SI-GRABADO-SIDC                      VALUE 'G'.
    05 SW-SQLCODE        PIC S9(4).
       88 RETORNO-OK                    VALUE 0.
       88 NO-ENCONTRADO                 VALUE +100.
       88 VARIOS-REGISTROS              VALUE -811.
    05 SW-POLIZA-SEG     PIC X(1).
       88 SI-POLIZA-SEG                 VALUE 'S'.
       88 NO-POLIZA-SEG                 VALUE 'N'.
```
**Función de Negocio:** Control de estados y flujo del programa.
**Uso en el Proceso:** Gestión de estados de consulta, validación de retornos DB2 y control de pólizas segregadas.

#### 1.7 AUXIVALU - Funciones del Sistema (Línea 1286)
```cobol
01  AUXIVALU.
    05  TRANSAC1     PIC X(4).
    05  TRANSAC      PIC X(4).
    05  USUARIO      PIC X(3).
    05  PFSCONS.
        10           PIC X(4)  VALUE 'SALI'.
        10           PIC X(4)  VALUE 'ANUL'.
        10           PIC X(4)  VALUE 'MODI'.
        10           PIC X(4)  VALUE 'SPAG'.
        10           PIC X(4)  VALUE 'STAS'.
```
**Función de Negocio:** Definición de funciones disponibles en el sistema y control de transacciones.
**Uso en el Proceso:** Navegación entre pantallas y control de operaciones del usuario.

#### 1.8 Estructura de Garantías TGARA (Línea 1324)
```cobol
05  TGARA.
    10           PIC X(7)  VALUE 'DP    2'.
    10           PIC X(7)  VALUE 'R.CI  4'.
    10           PIC X(7)  VALUE 'S.OB  4'.
    10           PIC X(7)  VALUE 'ROBO  4'.
    10           PIC X(7)  VALUE 'ACCE  4'.
    10           PIC X(7)  VALUE 'OCUP  4'.
    10           PIC X(7)  VALUE 'PLUS  4'.
    10           PIC X(7)  VALUE 'SUPER 5'.
```
**Función de Negocio:** Catálogo de tipos de garantías disponibles en las pólizas.
**Uso en el Proceso:** Validación y presentación de coberturas aplicables al siniestro.

### Nivel 05 - Elementos de Datos

## 2. Documentación Detallada de Campos

### 2.1 Campos de Identificación de Siniestro

| Campo | Tipo | Longitud | Descripción | Valores Posibles | Validaciones |
|-------|------|----------|-------------|------------------|--------------|
| WS-FCURSINI-SCCM | 9(11) | 11 | Número único de siniestro | 1-99999999999 | Debe existir en tabla TSIFECUR |
| WS-FCURPOLI-SCCM | 9(9) | 9 | Número de póliza | 1-999999999 | Debe existir en tabla de pólizas |
| WS-FCURMATR-SCCM | X(12) | 12 | Matrícula del vehículo | Alfanumérico | Formato según normativa DGT |

### 2.2 Campos de Clasificación

| Campo | Tipo | Longitud | Descripción | Valores Posibles | Validaciones |
|-------|------|----------|-------------|------------------|--------------|
| WS-FCURTSIN-SCCM | 9(2) | 2 | Tipo de siniestro | 01-99 | Debe existir en tabla TSITIPSI |
| WS-FCURCULP-SCCM | 9(1) | 1 | Código de culpabilidad | 0-9 | 0=Sin determinar, 1=Culpa propia, 2=Culpa tercero |
| WS-FCURSITU-SCCM | X(1) | 1 | Situación del siniestro | A,P,T,R,X | A=Abierto, P=Pendiente, T=Tramitado, R=Recobrable, X=Archivado |

### 2.3 Campos de Fechas

| Campo | Tipo | Longitud | Descripción | Valores Posibles | Validaciones |
|-------|------|----------|-------------|------------------|--------------|
| WS-FCURFEAC-SCCM | X(10) | 10 | Fecha de accidente | DD.MM.AAAA | Formato DD.MM.AAAA, no puede ser superior a fecha actual |
| WS-DIA-10 | 9(2) | 2 | Día del mes | 01-31 | Válido según mes y año |
| WS-MES-10 | 9(2) | 2 | Mes | 01-12 | Rango válido |
| WS-ANIO-10 | 9(4) | 4 | Año | 1900-2099 | Rango de años válidos |

### 2.4 Campos de Franquicia

| Campo | Tipo | Longitud | Descripción | Valores Posibles | Validaciones |
|-------|------|----------|-------------|------------------|--------------|
| WS-FCURFRAN-SCCM | X(1) | 1 | Tipo de franquicia | S,N,F | S=Sí, N=No, F=Flexible |
| WS-FCURFRA2-SCCM | X(1) | 1 | Franquicia secundaria | S,N | S=Sí, N=No |

### 2.6 Campos de Control y Estado

| Campo | Tipo | Longitud | Descripción | Valores Posibles | Validaciones |
|-------|------|----------|-------------|------------------|--------------|
| SW-VISIONADO-SIDC | X(1) | 1 | Estado de consulta SIDC | N,P,V,G | N=No consultar, P=Pendiente, V=Visto, G=Grabado |
| SW-SQLCODE | S9(4) | 4 | Código retorno DB2 | 0,+100,-811 | 0=OK, +100=No encontrado, -811=Varios registros |
| SW-POLIZA-SEG | X(1) | 1 | Indicador póliza segregada | S,N | S=Sí segregada, N=No segregada |
| TRANSAC1 | X(4) | 4 | Código transacción anterior | Alfanumérico | Debe existir en tabla de transacciones |
| TRANSAC | X(4) | 4 | Código transacción actual | Alfanumérico | Debe existir en tabla de transacciones |
| USUARIO | X(3) | 3 | Código de usuario | Alfanumérico | Debe existir en tabla de usuarios |

### 2.7 Campos de Funciones del Sistema

| Campo | Tipo | Longitud | Descripción | Valores Posibles | Validaciones |
|-------|------|----------|-------------|------------------|--------------|
| PFSCONS | Multiple | Variable | Funciones disponibles | SALI,ANUL,MODI,etc | Según tabla de funciones |
| LGARA | X(6) | 6 | Literal de garantía | DP,R.CI,S.OB,etc | Debe existir en catálogo de garantías |
| LGARL | 9(1) | 1 | Longitud del literal | 2-5 | Según tipo de garantía |

**Detalle de Funciones PFSCONS:**
- SALI: Salir del programa
- ANUL: Anular siniestro
- MODI: Modificar datos
- SPAG: Consultar pagos
- STAS: Consultar tasaciones
- TIMP: Tramitación de importes
- FIVA: Consultas FIVA (añadido 2021)
- SPRJ: Procedimientos judiciales

## 3. Contexto de Uso en Programas

### 3.1 Módulos que Utilizan la Copybook

#### Módulos Principales:
- **PDBSCCM:** Gestión de contrarios y comunicación con módulos externos
- **MDUQFCUR/MDUQPO9X:** Consulta y actualización de datos de siniestros
- **MDDSILOG:** Registro de auditoría de cambios
- **PDBSAFR:** Gestión de avalistas y franquicias
- **PDBSPJ0:** Tratamiento de procedimientos judiciales
- **MDSIFRFL:** Cálculo de franquicias flexibles
- **MDSLCALI:** Módulo de calificación de lesionados
- **MDDSSPAC:** Notificaciones y comunicaciones

#### Módulos de Acceso a Datos:
- **MDATPRX/MDDATPRX:** Recuperación de datos de productos
- **MDAFSEGX:** Gestión de pólizas segregadas
- **MDATAUCX:** Acceso a datos de colectivos
- **MDDGO005:** Acceso a datos del mediador
- **MDAPSOCX:** Datos de socios

### 3.2 Tablas DB2 Relacionadas

#### Tablas Principales de Siniestros:
| Tabla | Propósito | Relación |
|-------|-----------|----------|
| TSIFECUR | Fechas de curso de siniestros | Tabla principal |
| SIFECU2 | Datos adicionales de siniestros | Extensión de SIFECUR |
| TSLELESI | Lesionados por siniestro | Detalle por WS-FCURSINI-SCCM |
| TSLEPROV | Provisiones de lesionados | Detalle por LESISINI + LESIORDE |
| TSDMCMED | Costes medios | Cálculo de reservas |
| SDMRESV | Reservas vigentes | Gestión financiera |

#### Tablas de Configuración:
| Tabla | Propósito | Campos Relacionados |
|-------|-----------|---------------------|
| TSITIPSI | Tipos de siniestros | WS-FCURTSIN-SCCM |
| SITIPCO | Tipos auxiliares | WS-FCURTSIN-SCCM |
| SIPROTI | Siniestros por producto | WS-FCURTSIN-SCCM |
| SIGARCO | Coberturas por garantías | Garantías del siniestro |

#### Tablas de Auditoría y Control:
| Tabla | Propósito | Uso |
|-------|-----------|-----|
| SLELOGB | Log de operaciones | Auditoría de cambios |
| SGSTRAM | Tramitación SGE | Control de expedientes |
| SGSEXPE | Expedientes SGE | Estado de tramitación |

### 3.3 Operaciones Principales

1. **Consulta de Siniestros:** Los campos se utilizan para recuperar información completa del siniestro
2. **Actualización:** Modificación de estados y clasificaciones
3. **Auditoría:** Registro de cambios para control y seguimiento
4. **Intercambio de Datos:** Comunicación entre diferentes módulos del sistema
5. **Cálculo de Franquicias:** Determinación de franquicias flexibles
6. **Gestión de Reservas:** Cálculo y actualización de reservas financieras
7. **Notificaciones:** Envío de avisos y comunicaciones automáticas

## 4. Ejemplos de Registros

### 4.1 Ejemplo de Siniestro Completo
```
WS-FCURSINI-SCCM: 20240001234    // Número de siniestro año 2024
WS-FCURPOLI-SCCM: 123456789      // Póliza del cliente
WS-FCURMATR-SCCM: 1234ABC        // Matrícula del vehículo
WS-FCURFEAC-SCCM: 15.03.2024     // Fecha del accidente
WS-FCURTSIN-SCCM: 01             // Colisión frontal
WS-FCURCULP-SCCM: 2              // Culpa de tercero
WS-FCURSITU-SCCM: A              // Siniestro abierto
WS-FCURFRAN-SCCM: S              // Con franquicia
WS-FCURDANO-SCCM: 15             // Daños materiales graves
```

### 4.3 Ejemplo de Control de Flujo
```
SW-VISIONADO-SIDC: P             // Pendiente de ver datos SIDC
SW-SQLCODE: 0                    // Operación DB2 exitosa
TRANSAC: MODI                    // Función de modificación activa
USUARIO: USR                     // Usuario del sistema
```

### 4.4 Ejemplo de Estructura de Garantías
```
TGARA Elemento 1: DP    2        // Daños Propios, longitud 2
TGARA Elemento 2: R.CI  4        // Responsabilidad Civil, longitud 4
TGARA Elemento 3: ROBO  4        // Robo, longitud 4
```

## 5. Diagrama de Estructura de Datos

```
Offset  Campo                    Tipo      Longitud  Descripción
------  ----------------------   -------   --------  ------------------
0001    WS-GARANTIA-3           X(1)      1         Indicador garantía
0002    WS-FCURFEAC-SCCM        X(10)     10        Fecha accidente
0012    WS-FCURCULP-SCCM        9(1)      1         Culpabilidad
0013    WS-FCURTSIN-SCCM        9(2)      2         Tipo siniestro
0015    WS-FCURMATR-SCCM        X(12)     12        Matrícula
0027    WS-FCURSINI-SCCM        9(11)     11        Número siniestro
0038    WS-FCURFRAN-SCCM        X(1)      1         Franquicia principal
0039    WS-FCURFRA2-SCCM        X(1)      1         Franquicia secundaria
0040    WS-FCURTIMP-SCCM        9(1)      1         Tipo importe
0041    WS-FCURDANO-SCCM        9(2)      2         Tipo daño
0043    WS-FCURPOLI-SCCM        9(9)      9         Número póliza
0052    WS-FCURSITU-SCCM        X(1)      1         Situación
0053    WS-FCURPOCU-SCCM        9(2)      2         Porcentaje ocupación
0055    WS-FCURCCIA-SCCM        9(3)      3         Compañía
0058    WS-FCURCONV-SCCM        9(1)      1         Convenio
```

## 6. Glosario de Términos

| Término | Significado |
|---------|-------------|
| FCUR | Fichero de Curriculums/Siniestros |
| SINI | Siniestro |
| POLI | Póliza |
| MATR | Matrícula |
| FEAC | Fecha de Accidente |
| CULP | Culpabilidad |
| TSIN | Tipo de Siniestro |
| FRAN | Franquicia |
| TIMP | Tipo de Importe |
| DANO | Daño |
| SITU | Situación |
| POCU | Porcentaje de Ocupación |
| CCIA | Compañía |
| CONV | Convenio |
| SCCM | Sistema de Contrarios y Clientes |
| SIDC | Sistema de Control de Siniestros |
| LESI | Lesionado |
| PROV | Provisión |
| GARA | Garantía |
| DP | Daños Propios |
| R.CI | Responsabilidad Civil |
| S.OB | Seguro Obligatorio |
| ROBO | Robo del vehículo |
| ACCE | Accesorios |
| OCUP | Ocupantes |
| PLUS | Plus de garantías |
| SUPER | Súper garantías |
| SGE | Sistema de Gestión de Expedientes |
| FIVA | Sistema de consultas específico |
| ISPAC | Sistema de notificaciones |
| MRS | Sistema de mensajería |
| CAS | Sistema de comunicación externa |
| PJ | Procedimiento Judicial |
| AFR | Avalistas y Franquicias |
| UCM | Consulta de conmutadores |

## 7. Casos de Prueba y Validación

### 7.1 Caso de Prueba 1: Siniestro Completo Válido
**Entrada:**
- WS-FCURSINI-SCCM: 20240001001
- WS-FCURPOLI-SCCM: 123456789
- WS-FCURFEAC-SCCM: 15.01.2024
- WS-FCURTSIN-SCCM: 01
- WS-FCURSITU-SCCM: A

**Resultado Esperado:** Procesamiento exitoso, registro creado/actualizado
**Validación:** Verificar existencia en tabla TSIFECUR

### 7.2 Caso de Prueba 2: Campos Nulos o Vacíos
**Entrada:**
- WS-FCURSINI-SCCM: 0
- WS-FCURFEAC-SCCM: SPACES
- WS-FCURPOLI-SCCM: 0

**Resultado Esperado:** Error de validación
**Validación:** Mensaje de error específico para campos obligatorios

### 7.3 Caso de Prueba 3: Valores Máximos
**Entrada:**
- WS-FCURSINI-SCCM: 99999999999
- WS-FCURPOLI-SCCM: 999999999
- WS-FCURTSIN-SCCM: 99

**Resultado Esperado:** Procesamiento exitoso si los valores existen en tablas maestras
**Validación:** Verificar límites máximos y existencia en tablas de referencia

### 7.4 Caso de Prueba 4: Franquicia Flexible
**Entrada:**
- WS-FCURFRAN-SCCM: F
- WS-FCURFRA2-SCCM: N
- MDSIFRFL activado

**Resultado Esperado:** Cálculo correcto de descuentos de franquicia
**Validación:** Verificar tabla SPDESFF y cálculos de módulo MDSIFRFL

### 7.5 Caso de Prueba 5: Control de Estados
**Entrada:**
- SW-VISIONADO-SIDC: P
- SW-SQLCODE: +100
- SW-POLIZA-SEG: S

**Resultado Esperado:** Manejo correcto de estados y flujo del programa
**Validación:** Verificar transiciones de estado y mensajes apropiados

### 7.6 Caso de Prueba 6: Validación de Fechas
**Entrada:**
- WS-FCURFEAC-SCCM: 32.13.2024 (fecha inválida)
- WS-FEAC-10: 29.02.2023 (año no bisiesto)

**Resultado Esperado:** Error de validación de fecha
**Validación:** Mensaje de error de formato de fecha

### 7.7 Caso de Prueba 7: Garantías y Coberturas
**Entrada:**
- Siniestro con múltiples garantías
- TGARA con diferentes tipos

**Resultado Esperado:** Correcta identificación y procesamiento de garantías
**Validación:** Verificar tabla SIGARCO y aplicación de coberturas

## 8. Revisión con Expertos de Negocio

### 8.1 Áreas de Revisión Recomendadas
1. **Validación de Reglas de Negocio:** Confirmar con analistas funcionales las reglas de validación para cada campo
2. **Códigos de Estado:** Revisar con el área de siniestros los valores válidos para situaciones y tipos
3. **Franquicias:** Validar con el área comercial los tipos de franquicia y su aplicación
4. **Auditoría:** Confirmar con el área de control los requisitos de trazabilidad

### 8.2 Stakeholders Clave
- Analistas Funcionales de Siniestros
- Área de Control y Auditoría
- Responsables de Producto (Franquicias)
- Equipo de Desarrollo de Interfaces

## 9. Mantenimiento y Actualización

### 9.1 Cómo Actualizar esta Documentación

#### Pasos para Incorporar Nuevos Campos:
1. Identificar la nueva estructura de datos en el código COBOL
2. Actualizar la sección "División de Registros en Niveles Lógicos"
3. Añadir la documentación detallada del campo en la sección correspondiente
4. Actualizar el diagrama de estructura de datos con los nuevos offsets
5. Añadir términos al glosario si es necesario
6. Crear casos de prueba para los nuevos campos

#### Pasos para Eliminar Campos Obsoletos:
1. Marcar el campo como obsoleto con fecha
2. Mantener la documentación por un período de gracia (6 meses)
3. Eliminar completamente tras confirmación de no uso
4. Actualizar diagramas y casos de prueba

### 9.2 Convenciones de Versionado
- **Formato:** AAAAMMDD/AUTOR - DESCRIPCIÓN_CAMBIO
- **Fecha:** Fecha de la modificación
- **Autor:** Código del desarrollador/analista
- **Descripción:** Resumen breve del cambio realizado

#### Ejemplo:
```
20240315/XXJPEREZ - Añadido campo WS-FCURNUEVO para nueva funcionalidad
20240320/XXMLOPEZ - Actualizada validación de WS-FCURTSIN-SCCM
```

### 9.3 Control de Versiones
- Mantener historial de cambios en la cabecera del documento
- Incluir fecha, autor y descripción de cada modificación
- Revisar anualmente la vigencia de la documentación
- Coordinar cambios con el equipo de desarrollo y analistas funcionales

---

**Última Actualización:** 2024-01-15  
**Autor:** Sistema de Documentación Automática  
**Versión:** 1.0  
**Estado:** Vigente