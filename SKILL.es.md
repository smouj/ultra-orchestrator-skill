name: Ultra Orchestrator
description: Plataforma de orquestación y automatización de seguridad impulsada por IA para operaciones de seguridad empresarial
version: "2.1.0"
maintainer: Security Team <security@company.com>
tags:
  - security
  - ai
  - automation
  - orchestration
  - incident-response
platforms:
  - linux
  - darwin
  - windows
---

# Ultra Orchestrator

## Propósito

Ultra Orchestrator es una plataforma de orquestación de seguridad impulsada por IA diseñada para automatizar y coordinar operaciones de seguridad en infraestructura híbrida. Se integra con herramientas de seguridad empresarial, realiza análisis de amenazas inteligentes y ejecuta flujos de trabajo de respuesta automatizados con aprobación humana para acciones críticas.

Casos de uso reales:
- **Gestión automatizada de vulnerabilidades**: Programar escaneos con Nessus/OpenVAS, puntuación de riesgo por IA de hallazgos, creación automática de tickets JIRA y desencadenar implementación de parches a través de Ansible
- **Respuesta a incidentes en tiempo real**: Detectar anomalías mediante modelos ML, ejecutar playbooks de contención (aislar VLANs, bloquear IPs, rotar credenciales) y notificar automáticamente a las partes interesadas
- **Automatización del cumplimiento**: Monitoreo continuo del cumplimiento contra CIS, PCI-DSS, HIPAA; generar paquetes de evidencia con registros de auditoría inmutables
- **Enriquecimiento de inteligencia de amenazas**: Búsqueda automática de IOCs en VirusTotal, AlienVault OTX y fuentes de amenazas internas; correlacionar con alertas SIEM
- **Paneles de estado de seguridad**: Agregar hallazgos de más de 50 herramientas en puntuación de riesgo unificada con resúmenes ejecutivos generados por IA
- **Orquestación de rotación de credenciales**: Rotar más de 10,000 contraseñas de bases de datos/servicios en programa de 30 días usando HashiCorp Vault + Ansible
- **Coordinación de implementación de parches**: Agrupación inteligente de parches basada en gráfico de dependencias CB, ventanas de mantenimiento y validación de reversión
- **Detección de desviación de configuración de seguridad**: Comparar estado actual con benchmarks CIS; corrección automática de configuraciones no conformes

## Alcance

Ultra Orchestrator opera dentro del dominio de seguridad y proporciona estos comandos específicos:

### Comandos Principales
- `ultra scan [target] [options]` - Ejecutar escaneos de seguridad con análisis IA
- `ultra analyze [data-source] [--ai]` - Análisis de seguridad impulsado por IA
- `ultra playbook [execute|list|create|approve] [name]` - Gestión de playbooks de respuesta a incidentes
- `ultra compliance [check|report|evidence] [framework]` - Evaluación de cumplimiento y recopilación de evidencia
- `ultra threat-intel enrich [indicator] [--sources]` - Búsqueda y correlación de inteligencia de amenazas
- `ultra remediate [issue-id] [--dry-run]` - Remediation automatizada con capacidad de reversión
- `ultra monitor [start|stop|status|alerts]` - Monitoreo continuo de seguridad y alertas
- `ultra report generate [type] [--format]` - Reportes de seguridad (ejecutivo, técnico, cumplimiento)
- `ultra credential rotate [service] [--emergency]` - Gestión de rotación de credenciales
- `ultra config [get|set|validate|rollback]` - Gestión de configuración
- `ultra status [--detailed]` - Verificación de salud del sistema y componentes
- `ultra ticket [create|update|escalate]` - Integración con ITSM (JIRA, ServiceNow)
- `ultra backup [create|verify|restore]` - Operaciones de backup y restauración

### Subcomandos y Flags
```bash
# Escaneo de red con puntuación de riesgo IA
ultra scan network \
  --range 10.0.0.0/16 \
  --exclude 10.0.0.1,10.0.0.2 \
  --ports 22,80,443,3389 \
  --engine openvas \
  --ai-risk-score \
  --model risk-v2 \
  --credentials-file /etc/ultra/scan-creds.yaml \
  --output-format json \
  --report-path /var/reports/scan-$(date +%Y%m%d-%H%M%S).json \
  --compliance cis,hipaa \
  --ticket SEC-$(date +%Y%m%d)-001 \
  --notify slack:#security-alerts,email:secops@company.com

# Escaneo de aplicaciones web con análisis de rutas de ataque impulsado por IA
ultra scan web \
  --target https://app.example.com \
  --deep \
  --auth-file /etc/ultra/web-auth.json \
  --ai-attack-paths \
  --sso-integration okta \
  --rate-limit 10 \
  --user-agent "UltraOrchestrator/2.1"

# Análisis de logs con detección de anomalías ML
ultra analyze logs \
  --source /var/log/auth.log,/var/log/secure \
  --ai-anomalies \
  --model anomaly-detection-v3 \
  --time-range "24h" \
  --threshold 0.85 \
  --baseline /var/lib/ultra/baselines/auth-baseline.json \
  --alert-slack https://hooks.slack.com/services/xxx \
  --output /reports/anomalies-$(date +%Y%m%d).json

# Ejecución de playbook de respuesta a incidentes
ultra playbook execute ransomware-response \
  --target-servers db01,app01,web01 \
  --containment immediate \
  --preserve-evidence \
  --forensic-mode \
  --notify legal-team,exec-team,soc-team \
  --ticket IR-2024-001 \
  --approval required:manager \
  --timeout 4h \
  --post-verification

# Evaluación de cumplimiento con recopilación de evidencia
ultra compliance check pci-dss \
  --level 3 \
  --output html,json,csv \
  --evidence-dir /var/lib/ultra/evidence/pci-dss-20240301 \
  --sign-off required:compliance-officer \
  --auto-remediate false \
  --report-email compliance@company.com \
  --slack-notification #compliance

# Pipeline de enriquecimiento de inteligencia de amenazas
ultra threat-intel enrich \
  --file /tmp/iocs.csv \
  --sources virus-total,alien-vault,internal-threats,mandiant \
  --ai-correlation \
  --confidence-threshold 0.7 \
  --output /var/lib/ultra/threat-intel/enriched-$(date +%Y%m%d).json \
  --auto-block \
  --ticket THREAT-$(date +%Y%m%d)-001 \
  --ttl 30d

# Remediation automatizada con plan de reversión
ultra remediate CVE-2024-0001 \
  --targets web01,web02,db01 \
  --patch-current \
  --reboot-strategy staged \
  --rollback-snapshot /backups/ultra/pre-remediate-$(date +%Y%m%d)/ \
  --verify-services \
  --health-check-url https://healthchecks.io/ultra/xxx \
  --notify dev-team,secops \
  --maintenance-window "02:00-06:00"

# Monitoreo continuo con detección de anomalías IA
ultra monitor start \
  --name production-monitor \
  --sources /var/log/nginx/access.log,/var/log/mysql/error.log,/var/log/secure \
  --ai-model anomaly-detection-v3 \
  --alert-threshold high,medium \
  --slack-webhook https://hooks.slack.com/services/xxx \
  --email-alerts secops@company.com \
  --pagerduty-integration \
  --rate-limit 100 \
  --retention 90d \
  --baseline-update daily

# Generación de reportes de seguridad
ultra report generate executive \
  --period 30d \
  --format pdf,html \
  --template executive-summary-v3 \
  --ai-summary \
  --include-charts \
  --recipients executive@company.com,board@company.com \
  --password-protect \
  --classification internal
```

## Proceso de Trabajo

### 1. Inicialización del Sistema y Validación
```bash
# Inicializar Ultra Orchestrator con validación de configuración completa
sudo ultra init \
  --config /etc/ultra-orchestrator/config.yaml \
  --secrets /etc/ultra-orchestrator/secrets/ \
  --ca-cert /etc/ultra/ssl/ca.pem \
  --force-checks \
  --dry-run

# Salida esperada:
# [INIT] Cargando configuración desde /etc/ultra-orchestrator/config.yaml
# [VALIDATE] Versión de esquema: 2.1
# [CHECK] Dependencias Python: OK (46/46)
# [CHECK] Daemon Docker: ejecutándose (versión 24.0)
# [CHECK] Herramientas externas: OpenVAS (OK), Nessus (OK), Splunk (OK), Vault (OK)
# [AI] Cargando modelo de riesgo: risk-v2.1 (sha256: abc123...)
# [AI] Cargando modelo de anomalía: anomaly-detection-v3.0 (sha256: def456...)
# [DB] Conexión PostgreSQL: OK (versión 14.5)
# [NETWORK] Puertos requeridos abiertos: 8200, 9390, 8834, 8088, 9200
# [READY] Ultra Orchestrator inicializado exitosamente
```

### 2. Flujo de Trabajo de Gestión de Vulnerabilidades (Producción)
```bash
# Paso 1: Crear definición de objetivo de escaneo
ultra scan target create prod-web-servers \
  --hosts web01,web02,web03,web04 \
  --ports 80,443,8080,8443 \
  --credentials vault:secret/data/scan-creds/web \
  --exclude-ports 22,3389 \
  --rate-limit 50 \
  --parallel 4

# Paso 2: Programar escaneo consciente de cumplimiento
ultra scan schedule prod-weekly \
  --target prod-web-servers \
  --engine openvas \
  --schedule "0 2 * * 1" \  # Lunes 2AM
  --ai-risk-score \
  --compliance cis-level2,pci-dss,hipaa \
  --ticket-prefix SEC-WEEKLY \
  --maintenance-window "02:00-06:00" \
  --notify slack:#security-scans,email:secops@company.com

# Paso 3: Disparar manualmente con flag de emergencia
ultra scan run prod-weekly \
  --now \
  --ticket SEC-EMERGENCY-20240301 \
  --priority critical \
  --notify pagerduty \
  --escalate-after 2h

# Paso 4: Análisis y priorización IA
ultra analyze vulnerabilities \
  --scan-id scan-$(date +%Y%m%d)-001 \
  --ai-model risk-v2 \
  --cvss-threshold 7.0 \
  --exploitability-check \
  --asset-criticality-weight 2.0 \
  --output /var/reports/ai-analysis-$(date +%Y%m%d).json

# Paso 5: Generar automáticamente tickets de remediación con vinculación de playbook
ultra ticket create \
  --from-analysis /var/reports/ai-analysis-$(date +%Y%m%d).json \
  --template jira-security \
  --auto-assign team:web-patch \
  --due-date $(date -d "+7 days" +%Y-%m-%d) \
  --link-playbook patch-deployment-v2 \
  --escalation-rules \
  --slack-notification

# Paso 6: Ejecutar remediación automatizada (si se aprueba)
ultra remediate batch-001 \
  --ticket SEC-WEEKLY-001 \
  --playbook patch-deployment-v2 \
  --pre-check health-check \
  --dry-run \
  --approval required:secops-manager \
  --window "02:00-04:00"

# Si se aprueba:
ultra remediate batch-001 --apply --watch
```

### 3. Flujo de Trabajo de Respuesta a Incidentes (Escenario de Brecha de Datos)
```bash
# Paso 1: Disparar desde alerta SIEM (automático)
# SIEM envía webhook a Ultra Orchestrator
ultra playbook execute data-breach \
  --target db-prod-01,app-prod-01 \
  --severity critical \
  --ticket IR-2024-001 \
  --evidence-dir /forensics/IR-2024-001 \
  --chain-of-custody \
  --notify legal,exec,cfo,pr \
  --slack-channels #incidents,#legal \
  --pagerduty-escalation \
  --law-enforcement notify

# Paso 2: Ejecución de playbook con guía IA
[PLAYBOOK] Cargando 'data-breach' v4.2 (certificado)
[PASO 1/20] Aislar segmento de red (vlan 150) ✓ - 0m 12s
[PASO 2/20] Crear volcados de memoria forense (LiME) ✓ - 2m 34s
[PASO 3/20] Preservar logs (retención 30 días) ✓ - 0m 08s
[PASO 4/20] Bloquear IOCs en firewall ✓ - 0m 03s
[PASO 5/20] Rotar credenciales de base de datos (Vault) ✓ - 1m 22s
[PASO 6/20] Habilitar registro de auditoría a nivel de aplicación ✓ - 0m 15s
[ANÁLISIS IA] Detección de anomalías: Patrón de exportación de datos inusual detectado
[RECOMENDACIÓN IA] Considerar: Verificar exfiltración de datos a IP externa 185.220.101.45
[PASO 7/20] Capturar datos de flujo de red (NetFlow) ✓ - 1m 45s
[...]
[PASO 20/20] Generar reporte de incidente ✓ - 3m 22s
[COMPLETADO] Incidente contenido. Evidencia preservada en /forensics/IR-2024-001
[SIGUIENTE] Se recomienda fase de análisis forense

# Paso 3: Análisis post-incidente con IA
ultra analyze incident IR-2024-001 \
  --ai-root-cause \
  --timeline-reconstruction \
  --lessons-learned \
  --update-playbooks \
  --ticket IRR-2024-001

# Paso 4: Generar paquete de evidencia de cumplimiento
ultra compliance evidence collect \
  --framework nist-800-61 \
  --incident IR-2024-001 \
  --audit-trail /var/log/ultra/audit.log \
  --chain-of-custody \
  --sign-off required:cio,legal \
  --output /evidence/IR-2024-001-compliance.zip
```

### 4. Monitoreo Continuo de Cumplimiento
```bash
# Paso 1: Definir línea base de marco de cumplimiento
ultra compliance framework import cis-ubuntu-22.04 \
  --source https://www.cisecurity.org/benchmark/ubuntu_linux \
  --version 2.0.0 \
  --levels 1,2 \
  --controls 1.1,2.3,4.5,6.2

# Paso 2: Programar verificaciones automatizadas diarias
ultra compliance schedule cis-daily \
  --framework cis-ubuntu-22.04 \
  --targets all-linux-servers \
  --cron "0 3 * * *" \
  --ai-gap-analysis \
  --auto-remediate low,medium \
  --critical-approval required:compliance-manager \
  --evidence-capture \
  --report-format html,json

# Paso 3: Ejecutar verificación de cumplimiento bajo demanda
ultra compliance check cis-ubuntu-22.04 \
  --servers web01,db01,app01 \
  --levels 1,2 \
  --controls-all \
  --evidence-dir /var/lib/ultra/evidence/cis-20240301 \
  --ssh-key /etc/ultra/ssh/compliance-key \
  --parallel 10 \
  --timeout 2h

# Paso 4: Generar reporte de cumplimiento
ultra compliance report generate \
  --framework cis-ubuntu-22.04 \
  --period 30d \
  --format executive,technical,raw \
  --ai-summary \
  --charts compliance-trend,top-violations \
  --recipients compliance@company.com,audit@company.com \
  --password-protect \
  --send-signed

# Salida de muestra:
# Reporte de Evaluación CIS Ubuntu 22.04 Nivel 1+2
# Periodo: 2024-02-01 a 2024-03-01
# Controles Totales: 295
# Cumplidos: 273 (92.5%)
# No Cumplidos: 22
#   - Alto: 3 (1.0%)
#   - Medio: 9 (3.1%)
#   - Bajo: 10 (3.4%)
# Resumen IA: "Cumplimiento general mejoró 5% desde el último mes. 
# Problemas principales: 1.5.1 (config sshd), 4.2 (reglas auditd), 6.1.3 (syslog)."
```

### 5. Pipeline de Enriquecimiento de Inteligencia de Amenazas
```bash
# Paso 1: Ingerir IOCs desde múltiples fuentes
ultra threat-intel ingest \
  --source alien-vault-otx \
  --pulse-id 5f8d1a2b3c4d5e6f \
  --update-frequency 15m \
  --output /var/lib/ultra/threat-intel/otx-pulse.json

# Paso 2: Enriquecer IOCs existentes con múltiples fuentes
ultra threat-intel enrich \
  --file /tmp/new-iocs.csv \
  --sources virus-total,alien-vault,mandiant,intel471,internal \
  --ai-correlation \
  --confidence-threshold 0.8 \
  --ai-reputation-score \
  --geolocation \
  --timestamp-analysis \
  --output /var/lib/ultra/threat-intel/enriched-$(date +%Y%m%d).json \
  --ttl 90d

# Paso 3: Bloquear automáticamente IOCs maliciosos de alta confianza
ultra threat-intel block \
  --input /var/lib/ultra/threat-intel/enriched-$(date +%Y%m%d).json \
  --confidence >=0.9 \
  --targets firewall,proxy,siem \
  --rule-expiry 30d \
  --ticket THREAT-BLOCK-$(date +%Y%m%d)-001 \
  --dry-run \
  --approval required:secops-manager

# Paso 4: Correlacionar con eventos SIEM internos
ultra threat-intel correlate \
  --enriched-iocs /var/lib/ultra/threat-intel/enriched-$(date +%Y%m%d).json \
  --siem-source splunk \
  --query "index=security earliest=-24h" \
  --ai-timeline \
  --affected-assets \
  --output /reports/correlations-$(date +%Y%m%d).json

# Paso 5: Generar boletín de inteligencia de amenazas
ultra threat-intel bulletin create \
  --correlations /reports/correlations-$(date +%Y%m%d).json \
  --ai-summary \
  --affected-regions \
  --recommended-actions \
  --recipients security-team@company.com \
  --format html,pdf \
  --classification internal
```

### 6. Generación de Panel de Control de Seguridad con IA
```bash
# Generar panel ejecutivo con información de IA
ultra dashboard create \
  --name "Q1-2024-Security-Posture" \
  --period 90d \
  --ai-insights \
  --metrics vulnerability-trend,incident-response,compliance,threat-intel \
  --charts heat-map-risk,top-vulnerable-assets,incident-timeline \
  --benchmark industry:financial \
  --forecast 90d \
  --output /var/www/dashboards/q1-2024.html \
  --email-executive-summary \
  --password-protect \
  --share-slack #security-dashboards
```

## Reglas de Oro

1. **Nunca almacenar credenciales en texto plano** - Usar exclusivamente HashiCorp Vault; `ultra creds validate` debe pasar antes de cualquier escaneo
2. **Siempre usar dry-run para cambios en producción** - Añadir `--dry-run` a cualquier comando de remediación, configuración o eliminación
3. **Mantener registro de auditoría inmutable** - Todas las acciones registradas en `ULTRA_AUDIT_LOG` con firmas SHA256; retención 7 años
4. **Las decisiones de IA requieren validación humana para acciones críticas** - Establecer `ULTRA_AI_REVIEW_REQUIRED=true` para incidentes P1/P2; nunca auto-aprobar rotaciones de credenciales
5. **Segregar entornos estrictamente** - Los comandos de producción requieren flag `--env production`; staging por defecto `--env staging`; `ultra doctor` debe mostrar verde para prod
6. **Backup antes de cualquier remediación** - Snapshots automáticos creados cuando `ULTRA_AUTO_BACKUP=true` (por defecto); verificar con `ultra backup verify`
7. **Reversión obligatoria para todos los cambios** - Cada ticket de remediación debe incluir `--with-rollback` y `ultra remediate rollback --test`
8. **Escaneo consciente de cumplimiento** - Todos los escaneos deben incluir al menos un marco de cumplimiento: `--compliance cis,hipaa,pci`
9. **Aplicar límite de velocidad** - Respetar límites de herramientas; configurar `ULTRA_SCAN_RATE_LIMIT` (por defecto: 10 req/min); usar flag `--throttle`
10. **Integración de gestión de cambios** - Los cambios en producción requieren ID de ticket válido: `--ticket ITIL-#####`; rechazar sin `ultra ticket validate`
11. **Cifrar todos los datos en tránsito y en reposo** - TLS 1.3+ para todas las llamadas API; cifrado AES-256 para reportes almacenados
12. **Acceso con privilegio mínimo** - La cuenta de servicio Ultra usa `ultra-svc` con solo permisos sudo requeridos; sin login root
13. **Cobertura de monitoreo de seguridad** - Todos los activos críticos deben tener al menos un monitor activo: `ultra monitor list --critical-only`
14. **Preservación de evidencia** - Los playbooks de respuesta a incidentes deben preservar volcados de memoria, logs y capturas de red por 90 días

## Ejemplos

### Ejemplo 1: Escaneo de vulnerabilidad crítica y remediación automatizada
```bash
# Comando real de usuario
$ ultra scan run critical-cves --engine nessus --ai-risk-score --ticket SEC-2024-0789 --compliance pci-dss --dry-run

# Salida real del sistema (abreviada)
[INFO] Ultra Orchestrator v2.1.0 (commit: a1b2c3d)
[SCAN] Inicializando motor Nessus (host: scanner01.company.com:8834)
[AUTH] Validando credenciales desde Vault secret/data/nessus/creds ✓
[SCAN] Objetivo: 47 servidores de producción (rango: 10.1.0.0/16)
[AI] Cargando modelo de riesgo: risk-v2.1 (umbral de confianza: 0.85)
[SCAN] Iniciando escaneo de vulnerabilidades (política: full-audit, max-hosts: 50)
[PROGRESO] 0%...15%...42%...67%...89%...100%
[SCAN] Completado: 234 vulnerabilidades encontradas
[AI] Puntuación de riesgo: 12 críticas (CVSS ≥9.0), 34 altas (7.0-8.9), 89 medias
[AI] Principales hallazgos: CVE-2024-0001 (9.8) - Ejecución Remota de Código en nginx, CVE-2024-0002 (9.5) - Escalación de Privilegios
[COMPLIANCE] Mapeo PCI-DSS: 8/12 requisitos impactados
[TICKET] Crearía: tickets JIRA SEC-2024-0789-01 hasta SEC-2024-0789-12
[REMEDIATE] Ejecutaría playbook: patch-deployment-v2 (4 objetivos, estimado 2h)
[ROLLBACK] Crearía snapshot: /backups/ultra/pre-remediate-20240301-143022
[DRY-RUN] No se aplicaron cambios. Eliminar '--dry-run' para ejecutar.
```

### Ejemplo 2: Orquestación de respuesta a incidentes - Detección de Ransomware
```bash
# Comando real de usuario (disparado por alerta SIEM)
$ ultra playbook execute ransomware-detection --target db-prod-01,app-prod-01 --severity critical --ticket IR-2024-001 --chain-of-custody --notify legal,exec,cfo,soc

# Salida real del sistema
[IR] Respuesta a Incidentes inicializada: IR-2024-001
[PLAYBOOK] Cargando 'ransomware-detection' v5.1 (certificado: 2024-02-15)
[PASO 1/45] Validar fuente de alerta: Alerta Splunk "Posible ransomware: proceso encrypt*.exe" ✓
[PASO 2/45] Crear directorio de evidencia forense: /forensics/IR-2024-001/ (inmutable) ✓
[PASO 3/45] Aislar red: Añadiendo servidores a vlan 199 (cuarentena) ✓
[PASO 4/45] Capturar volcados de memoria vía módulo LiME ✓ /forensics/IR-2024-001/mem-db-prod-01.dump
[THREAT-INTEL] Consultando feed de amenazas interno para "encrypt*.exe" → Coincidencia: grupo de extorsión "BlackCat" TTP
[ANÁLISIS IA] Análisis comportamental: Árbol de procesos coincide con patrón de ransomware (confianza: 94%)
[RECOMENDACIÓN] Verificar: C:\Windows\Temp\random.tmp, conexiones de red a 185.220.101.0/24
[PASO 5/45] Preservar logs: /var/log/secure (retención: 365d) ✓
[PASO 6/45] Bloquear IOCs en firewall perimetral ✓ (4 IPs, 12 dominios)
[PASO 7/45] Rotar credenciales de base de datos usando Vault ✓ (27 cuentas de servicio rotadas)
[PASO 8/45] Habilitar modo forense en base de datos (réplicas de solo lectura) ✓
[PASO 9/45] Snapshot de todas las VMs afectadas ✓
[PASO 10/45] Notificar partes interesadas: legal (email+Slack), ejecutivos (SMS), CFO (email cifrado) ✓
[...]
[PASO 45/45] Generar reporte de incidente (borrador) ✓ /reports/IR-2024-001-draft.pdf
[COMPLETADO] Incidente contenido. Evidencia preservada. Siguiente: Fase de análisis forense.
[RESUMEN IA] "Ransomware variante 'BlackCat' detectada mediante análisis comportamental de procesos. 
Contención exitosa. Exfiltración de datos no detectada (logs de red sin salida >100MB)."
[ESCALACIÓN] Equipo legal solicitó notificación a fuerzas del orden. Acción requerida: `ultra incident notify law-enforcement --case IR-2024-001`
[SEGUIMIENTO] Playbook recomendado: post-incident-review (ejecutar después del análisis forense)"
```

### Ejemplo 3: Auditoría de cumplimiento - Recopilación de evidencia PCI-DSS
```bash
# Comando real de usuario
$ ultra compliance check pci-dss --level 3 --output html,evidence --ticket PCI-2024-0301 --sign-off required:ciso,auditor --dry-run

# Salida real del sistema
[COMPLIANCE] Evaluación PCI-DSS v4.0 (Nivel 3 - Todos los requisitos)
[ALCANCE] 52 sistemas en CDE (Entorno de Datos de Titulares)
[AI] Cargando modelo de cumplimiento: pci-dss-v4 (entrenado en 1,200 evaluaciones)
[CHECK] Requisito 1: Instalar y mantener configuraciones de firewall
  1.1.1 - Reglas de firewall documentadas: ✓ (12/12 controles conformes)
  1.2.2 - Cambios de firewall aprobados: ⚠ (3 cambios no aprobados detectados)
  1.3.2 - Segregación DMZ: ✓
[CHECK] Requisito 2: No usar valores por defecto de proveedor
  2.2.1 - Endurecimiento de sistema: ⚠ (5 servidores sin línea base CIS)
  2.2.2 - Acceso admin cifrado: ✓
[...]
[ANÁLISIS DE BRECHAS IA] 22 elementos no conformes total:
  Crítico: 2 (1.2.2, 8.2.3)
  Alto: 6 (2.2.1, 3.4.1, 4.1.1, 6.4.3, 10.5.2, 12.10.1)
  Medio: 7
  Bajo: 3
[REPORTE] Generaría:
  - Resumen ejecutivo: /reports/pci-dss-exec-20240301.html
  - Reporte técnico: /reports/pci-dss-tech-20240301.html
  - Paquete de evidencia: /evidence/pci-dss-20240301/ (234 archivos, 1.2GB)
  - Plan de remediación: /remediation/PCI-2024-0301-plan.md
[TICKET] Crearía ticket de auditoría: PCI-2024-0301 (JIRA)
[FIRMA] Requeriría: CISO, Auditor Externo
[DRY-RUN] No se recogió evidencia. Eliminar '--dry-run' para ejecutar.
```

### Ejemplo 4: Rotación automatizada de credenciales
```bash
# Comando real de usuario
$ ultra credential rotate all-services --schedule quarterly --emergency false --dry-run

# Salida real del sistema
[CREDENTIAL] Motor de rotación de credenciales Ultra v1.5
[INVENTARIO] 1,247 cuentas de servicio descubiertas en 4 entornos
[FILTRO] Elegibles para rotación (90+ días de antigüedad): 892 cuentas
[EXCLUSIONES] 47 cuentas excluidas (críticas-sistema, ldap-bind, legacy-apps)
[PLAN] Secuencia de rotación (rodante, sin downtime):
  - Lote 1: No críticas (02:00-04:00) - 412 cuentas
  - Lote 2: Semi-críticas (04:00-06:00) - 312 cuentas
  - Lote 3: Críticas (06:00-08:00) - 168 cuentas
[VAULT] Destino: Vault secret/data/credentials/prod/ (montaje: kv-v2)
[ANSIBLE] Playbook Ansible: rotate-credentials.yml (idempotente)
[PRE-CHECK] ¿Todos los servicios objetivo soportan hot-reload? 892/892 ✓
[BACKUP] Crearía snapshot de Vault: /backups/vault/creds-$(date +%Y%m%d).snap
[DRY-RUN] No se ejecutaron rotaciones. Para ejecutar:
  ultra credential rotate batch-1 --apply --window "02:00-04:00"
```

### Ejemplo 5: Automatización de inteligencia de amenazas - IOCs desde OSINT
```bash
# Comando real de usuario
$ ultra threat-intel enrich --file /tmp/iocs.csv --sources virus-total,alien-vault --ai-correlation --auto-block --ticket THREAT-2024-001

# Formato de archivo /tmp/iocs.csv:
# tipo,valor,fuente,confianza
# ip,185.220.101.45,alien-vault,0.95
# domain,malicious.example.com,virustotal,0.87
# hash,44d88612fea8a8f36c82d4,internal,0.99

# Salida real del sistema
[THREAT-INTEL] Motor de enriquecimiento v2.3
[ARCHIVO] Analizando /tmp/iocs.csv: 124 IOCs (23 duplicados)
[API] VirusTotal: 121 consultas (límite de tasa: 500/min) ✓
[API] AlienVault OTX: 121 consultas ✓
[API] Feed interno: 124 búsquedas ✓
[AI] Correlacionando IOCs con incidentes históricos...
[AI] ¡Correlación encontrada! IOC 185.220.101.45 vinculado a IR-2023-045 (ataque anterior)
[ENRIQUECIMIENTO] Resultados:
  - 87 IOCs confirmados maliciosos (confianza ≥0.85)
  - 32 IOCs sospechosos (0.7-0.85)
  - 5 IOCs benignos (<0.7)
[AUTO-BLOCK] Planificando reglas de firewall/proxy para 87 IOCs confirmados
[ROLLBACK] Generaría plan de reversión: /var/lib/ultra/rollback/threat-block-$(date +%Y%m%d).json
[TICKET] Crearía: THREAT-2024-001 (JIRA Security)
[APROBACIÓN] Requerida: Aprobación de SOC Manager para auto-bloqueo
[DRY-RUN] No se aplicó bloqueo. Para ejecutar:
  ultra threat-intel block /var/lib/ultra/threat-intel/enriched-$(date +%Y%m%d).json --apply
```

## Dependencias y Requisitos

### Requisitos del Sistema
- **SO**: Ubuntu 22.04 LTS+, RHEL 8+, Debian 12+, macOS 12+
- **CPU**: 8+ núcleos (16 recomendados para escaneo paralelo)
- **RAM**: 16GB mínimo, 32GB recomendado para modelos IA
- **Disco**: 200GB SSD (100GB para reportes, 50GB para evidencia, 50GB para modelos)
- **Red**: HTTPS saliente (443) a todas las APIs de herramientas de seguridad; entrante para agentes de monitoreo

### Entorno Python
```bash
# Versión de Python y paquetes requeridos
python3 --version  # >= 3.9.0, < 3.12
pip install -r requirements.txt

# Dependencias principales (requirements.txt)
ultra-orchestrator==2.1.0
ultra-ai-risk-engine>=1.5.0
ultra-playbook-engine>=2.0.0
vuln-scanner-client>=3.2.0      # Integración Nessus/OpenVAS
threat-intel-client>=1.8.0      # VT, OTX, Mandiant
jira-client>=3.0.0              # Integración JIRA
servicenow-client>=2.1.0        # Integración ServiceNow
slack-sdk>=3.0.0                # Notificaciones Slack
pagerduty>=2.2.0                # Integración PagerDuty
splunk-sdk>=1.7.0               # Splunk HEC
vault-auth>=0.9.0               # HashiCorp Vault
elasticsearch>=8.0.0            # Stack ELK
cryptography>=3.4               # Cifrado
pyyaml>=6.0                     # Análisis de config
jmespath>=0.10                  # Consulta JSON
pandas>=1.5                    # Análisis de datos (modelos IA)
numpy>=1.21                    # Computación numérica
scikit-learn>=1.2              # Modelos ML
torch>=1.13                    # Deep learning (opcional)
```

### Herramientas y Servicios Externos
```bash
# Escáneres de seguridad (uno o más)
openvas-scanner   # Greenbone Security Assistant (puerto 9390)
nessus            # Tenable Nessus (puerto 8834)
qualys            # Qualys Cloud Platform
aws-inspector     # Amazon Inspector

# SIEM y agregación de logs
splunk            # Splunk Enterprise/Cloud (puerto 8089, 8088)
elasticsearch     # Stack ELK (puerto 9200, 5044)
sumologic         # Sumo Logic
datadog           # Datadog

# Gestión de secretos
vault             # HashiCorp Vault (puerto 8200)

# Gestión de configuración
ansible           # Ansible (para playbooks de remediación)
salt              # SaltStack (alternativa)

# Integración ITSM
jira              # JIRA Service Management
servicenow        # ServiceNow

# Canales de notificación
slack             # Slack Workflow/Webhooks
pagerduty         # PagerDuty
twilio            # Notificaciones SMS (Twilio)
smtp              # Notificaciones email
mattermost        # Mattermost
teams             # Microsoft Teams

# Almacenamiento y backup
postgresql        # Base de datos PostgreSQL (ultra_db)
redis             # Caché Redis (opcional)
s3                # AWS S3 (almacenamiento de reportes)
minio             # MinIO (S3 auto-hospedado)
```

### Imágenes Docker (Componentes Containerizados)
```yaml
services:
  ultra-orchestrator:
    image: ultra/orchestrator:2.1.0
    ports:
      - "8080:8080"  # API
      - "9090:9090"  # Métricas
    volumes:
      - /etc/ultra:/config
      - /var/reports:/reports
      - /var/lib/ultra:/data
      - /backups:/backups
  
  ultra-ai-engine:
    image: ultra/ai-engine:1.5.0
    ports:
      - "5000:5000"  # Inferencia IA
    volumes:
      - /models:/models
  
  ultra-scanner:
    image: ultra/scanner:3.2.0
    cap_add:
      - NET_RAW  # Para operaciones nmap
```

### Archivos de Configuración
```bash
# Configuración principal
/etc/ultra-orchestrator/config.yaml

# Directorio de secretos (tokens Vault, claves API)
/etc/ultra-orchestrator/secrets/
  ├── vault-token
  ├── nessus-api-key
  ├── virus-total-api-key
  ├── slack-webhook
  └── jira-api-token

# Directorio de playbooks
/var/lib/ultra/playbooks/
  ├── ransomware-response.yml
  ├── patch-deployment.yml
  ├── data-breach.yml
  └── compliance-evidence.yml
```

### Variables de Entorno
```bash
# Configuración principal de Ultra
export ULTRA_HOME=/opt/ultra-orchestrator
export ULTRA_CONFIG=/etc/ultra-orchestrator/config.yaml
export ULTRA_LOG_LEVEL=INFO  # DEBUG, INFO, WARNING, ERROR, CRITICAL
export ULTRA_LOG_FILE=/var/log/ultra/ultra.log
export ULTRA_AUDIT_LOG=/var/log/ultra/audit.log

# Base de datos
export ULTRA_DB_HOST=postgresql.internal
export ULTRA_DB_PORT=5432
export ULTRA_DB_NAME=ultra_db
export ULTRA_DB_USER=ultra_user
export ULTRA_DB_PASSWORD_FILE=/etc/ultra/secrets/db-password

# Servicios externos
export VAULT_ADDR=https://vault.company.com:8200
export VAULT_TOKEN_FILE=/etc/ultra/secrets/vault-token
export VAULT_SSL_VERIFY=true
export VAULT_CA_CERT=/etc/ultra/ssl/vault-ca.pem

export NESSUS_HOST=scanner01.company.com
export NESSUS_PORT=8834
export NESSUS_ACCESS_KEY_FILE=/etc/ultra/secrets/nessus-access-key

export OPENVAS_HOST=openvas.company.com
export OPENVAS_PORT=9390
export OPENVAS_USER=ultra-scanner
export OPENVAS_PASSWORD_FILE=/etc/ultra/secrets/openvas-password

export SPLUNK_HEC_TOKEN_FILE=/etc/ultra/secrets/splunk-hec-token
export SPLUNK_HEC_URL=https://splunk.company.com:8088/services/collector

export JIRA_URL=https://jira.company.com
export JIRA_USER=ultra-bot
export JIRA_API_TOKEN_FILE=/etc/ultra/secrets/jira-token

# Modelos IA
export ULTRA_AI_MODEL_PATH=/models/risk-scoring,/models/anomaly-detection
export ULTRA_AI_MODEL_CACHE_SIZE=4096
export ULTRA_AI_BATCH_SIZE=32
export ULTRA_AI_CONFIDENCE_THRESHOLD=0.85
export ULTRA_AI_GPU_ENABLED=false  # Establecer true si hay GPU disponible

# Flags operacionales
export ULTRA_AUTO_BACKUP=true
export ULTRA_AUTO_UPDATE_CHECK=true
export ULTRA_DRY_RUN_DEFAULT=false
export ULTRA_AI_REVIEW_REQUIRED=true  # Aprobación humana para acciones IA
export ULTRA_SCAN_RATE_LIMIT=10  # solicitudes por minuto
export ULTRA_MAX_CONCURRENT_SCANS=4
export ULTRA_PLAYBOOK_TIMEOUT=7200  # segundos

# Canales de notificación
export SLACK_WEBHOOK_URL_FILE=/etc/ultra/secrets/slack-webhook
export PAGERDUTY_INTEGRATION_KEY_FILE=/etc/ultra/secrets/pagerduty-key
export EMAIL_SMTP_SERVER=smtp.company.com
export EMAIL_FROM=ultra-orchestrator@company.com
export EMAIL_ALERTS=secops@company.com

# Cumplimiento
export ULTRA_COMPLIANCE_EVIDENCE_RETENTION=3650  # días (10 años)
export ULTRA_AUDIT_RETENTION=2555  # días (7 años)
export ULTRA_CHAIN_OF_CUSTODY_ENABLED=true
```

## Pasos de Verificación

### Lista de verificación posterior a la instalación
```bash
# 1. Ejecutar diagnósticos completos del sistema
sudo ultra doctor --full --json

# Salida esperada (resumida):
{
  "status": "healthy",
  "components": {
    "python": {"version": "3.9.12", "status": "ok"},
    "database": {"type": "postgresql", "version": "14.5", "status": "ok"},
    "vault": {"connected": true, "version": "1.15.0"},
    "openvas": {"status": "running", "port": 9390, "status": "ok"},
    "nessus": {"status": "running", "port": 8834, "status": "ok"},
    "splunk": {"hec": true, "status": "ok"},
    "ai_models": [
      {"name": "risk-v2.1", "status": "loaded", "checksum": "abc123"},
      {"name": "anomaly-detection-v3", "status": "loaded", "checksum": "def456"}
    ],
    "ansible": {"version": "2.9.6", "status": "ok"},
    "disk_space": {"reports": "45%", "evidence": "62%", "models": "34%"},
    "network_ports": {"8200": "open", "9390": "open", "8834": "open", "8088": "open"}
  }
}
```

### 2. Probar integraciones externas
```bash
# Probar conectividad con Vault
ultra vault test-auth --secret secret/data/test

# Esperado: "Vault authentication successful (token TTL: 24h)"

# Probar conectividad de escáneres
ultra scan engines list --test

# Esperado:
# Engine         Status    Latency   Version
# openvas        OK        45ms      22.04
# nessus         OK        120ms     10.5.2

# Probar modelos IA
ultra ai models list --validate

# Esperado:
# Model Name              Version   Status    Validity
# risk-scoring            v2.1      OK        Valid until: 2024-12-31
# anomaly-detection       v3.0      OK        Valid until: 2025-06-30
# threat-correlation      v1.2      OK        Valid until: 2024-09-30

# Probar canales de notificación
ultra notify test --channel slack,email,pagerduty --template test-alert

# Esperado:
# [SLACK] Message sent to #security-testing (ts: 123456.789)
# [EMAIL] Sent to secops@company.com (message-id: <xxx>)
# [PAGERDUTY] Incident created: PTR-789abc (status: triggered)

# Probar sistema de tickets
ultra ticket validate --system jira --dry-run --template security-incident

# Esperado: "JIRA connection OK. Project SEC exists. Required fields: priority, severity, assignee"
```

### 3. Verificar sistemas de backup y reversión
```bash
# Probar creación de backup
ultra backup create --type full --dry-run

# Esperado: "[DRY-RUN] Would create backup: /backups/ultra/full-20240301-1430.tar.gz (size: 45.2GB)"

# Probar sistema de snapshots (si usa LVM/ZFS)
sudo ultra snapshot create test-snapshot --volume ultra_data

# Esperado: "Snapshot created: ultra_data-snap-test-snapshot (size: 0B, COW)"

# Probar generación de plan de reversión
ultra remediate generate-rollback CVE-2024-0001 --targets web01,web02

# Esperado: "Rollback plan written to: /var/lib/ultra/rollback/CVE-2024-0001-plan.json"
```

### 4. Verificar registro de auditoría
```bash
# Verificar integridad del log de auditoría
ultra audit verify --log /var/log/ultra/audit.log --since "2024-03-01"

# Esperado:
# [VERIFY] Log file: /var/log/ultra/audit.log (142,567 lines)
# [CHECK] SHA256 chain verified up to: entry #142567 (2024-03-01 14:23:01)
# [CHECK] No gaps or tampering detected.
# [SUMMARY] 89 audit entries in last 24h, all signed successfully.

# Probar rotación de logs de auditoría
sudo logrotate -vf /etc/logrotate.d/ultra-orchestrator

# Esperado: "logrotate: /var/log/ultra/audit.log"
```

### 5. Verificaciones de cumplimiento y seguridad
```bash
# Ejecutar auditoría de configuración de seguridad
ultra config audit --check security

# Esperado:
# [AUDIT] Security configuration check (89 controls)
# ✓ All passwords stored in Vault (not in config)
# ✓ TLS 1.3+ enforced for external APIs
# ✓ Audit logging enabled and immutable
# ✓ No default credentials in use
# ✓ RBAC properly configured
# ✓ API rate limiting active
# [PASS] 89/89 controls compliant

# Verificar asignaciones RBAC
ultra role list --users

# Salida esperada:
User          Roles               Expires
------------  ------------------  -----------
alice         operator,analyst   never
bob           admin              2024-12-31
service-acct scanner-engine     never
```

### 6. Configuración de monitoreo de salud
```bash
# Verificar endpoint de métricas
curl -s http://localhost:9090/metrics | grep ultra_ | head -10

# Esperado:
# ultra_scans_total{status="completed"} 1427
# ultra_incidents_total{severity="critical"} 12
# ultra_remediations_total{status="success"} 856
# ultra_ai_inference_duration_seconds_bucket{model="risk-v2"} 1024 2048 4096

# Verificar reglas de alertas de Prometheus
ultra alerts list --active

# Esperado:
# Alert: UltraScannerDown
#   Condition: up{job="ultra-scanner"} == 0
#   For: 5m
#   Severity: critical
#   Runbook: /var/lib/ultra/runbooks/scanner-down.md
```

## Solución de Problemas

### Problema: El escaneo falla con "Authentication failed for engine openvas"
**Síntomas**: `ultra scan run` falla inmediatamente con error de auth a pesar de credenciales válidas en Vault.

**Causas raíz**:
- Token Vault expirado
- Ruta de secreto incorrecta en configuración
- Contraseña de usuario OpenVAS cambiada
- Problema de conectividad de red

**Diagnóstico**:
```bash
# 1. Verificar validez del token Vault
vault token lookup -format=json | jq '.data.expire_time'

# 2. Probar recuperación de credenciales
vault kv get -field=password secret/data/scan-creds/openvas

# 3. Validar conexión OpenVAS manualmente
gvm-tool --hostname openvas.company.com --port 9390 --protocol=OSP \
  --certificate=/etc/ultra/ssl/ca.pem \
  --get-version

# 4. Verificar caché de credenciales de Ultra
ultra creds status --engine openvas
```

**Solución**:
```bash
# Si el token Vault expiró:
vault login -method=ldap username=ultra-svc

# Actualizar token en configuración Ultra:
echo $VAULT_TOKEN | sudo tee /etc/ultra/secrets/vault-token

# Si la contraseña OpenVAS cambió, actualizar secreto Vault:
vault kv put secret/data/scan-creds/openvas password="new-pass"

# Rotar caché de credenciales de Ultra:
ultra creds rotate --engine openvas --force

# Reintentar escaneo con debug:
ultra scan run test-scan --engine openvas --debug 2>&1 | grep -A5 "AUTH"
```

### Problema: La puntuación de riesgo IA devuelve "Unknown model" o "Model load failed"
**Síntomas**: El escaneo se completa pero el análisis IA falla: `[AI] Error: Model 'risk-v2' not found`.

**Causas raíz**:
- Archivos de modelo faltantes en `ULTRA_AI_MODEL_PATH`
- Desajuste de versión de modelo
- Archivos de modelo corruptos
- Incompatibilidad GPU/Torch

**Diagnóstico**:
```bash
# 1. Listar modelos disponibles
ultra ai models list --local --verbose

# Esperado:
# /models/risk-scoring/risk-v2.1/
#   model.pth (size: 2.1GB, sha256: abc123)
#   config.yaml
#   metadata.json

# 2. Verificar integridad del modelo
ultra ai models verify risk-v2.1 --checksum

# 3. Probar inferencia del modelo
ultra ai test risk-v2.1 --input /var/lib/ultra/test/vulnerability-sample.json

# 4. Verificar compatibilidad GPU/Torch
python3 -c "import torch; print(torch.__version__, torch.cuda.is_available())"
```

**Solución**:
```bash
# Descargar modelo faltante (del registro de modelos interno)
ultra ai models download risk-v2.1 \
  --source s3://ultra-models/risk-scoring/v2.1/ \
  --verify-checksum \
  --target /models/risk-scoring/

# O desde backup:
sudo cp -r /backups/models/risk-v2.1 /models/risk-scoring/

# Actualizar ruta de modelo en configuración:
ultra config set ai.model_path /models/risk-scoring,/models/anomaly-detection

# Si hay problemas con GPU, deshabilitar GPU en configuración:
ultra config set ai.gpu_enabled false

# Recargar configuración:
ultra config reload --validate
```

### Problema: La ejecución del playbook se queda atascada en el paso N indefinidamente
**Síntomas**: `ultra playbook status <incident-id>` muestra paso atascado sin progreso por >30 minutos.

**Causas raíz**:
- Timeout de API externa (Vault, Ansible, proveedor cloud)
- Partición de red
- Agotamiento de recursos (disco lleno, presión de memoria)
- Bucle infinito en paso de playbook personalizado
- Token de autenticación expirado en medio de ejecución

**Diagnóstico**:
```bash
# 1. Obtener estado detallado del paso
ultra playbook status <incident-id> --step 12 --debug

# Incluye:
# Paso: "rotate-credentials"
# Iniciado: 2024-03-01 14:23:01
# Comando: ansible-playbook -i inventory rotate.yml
# PID: 12345
# Estado: EJECUTANDO (2h 15m)

# 2. Verificar estado del proceso
sudo ps aux | grep ansible-playbook | grep 12345

# 3. Verificar recursos del sistema
free -h
df -h /var
dmesg | tail -20

# 4. Verificar logs del playbook
sudo tail -100 /var/log/ultra/playbooks/<incident-id>.log

# 5. Probar conectividad de servicios externos
ultra connectivity test --target vault,ansible-controller,aws

# 6. Verificar procesos zombies
ps aux | grep defunct
```

**Solución**:
```bash
# Si Ansible se colgó por host inalcanzable:
# Forzar matar el playbook (último recurso)
sudo kill -9 12345

# Marcar paso como fallido y continuar:
ultra playbook step fail <incident-id> 12 \
  --reason "Timeout de Ansible - host db01 inalcanzable" \
  --continue

# O abortar playbook completo:
ultra playbook abort <incident-id> \
  --reason "Intervención manual requerida - partición de red" \
  --preserve-state

# Para problemas de expiración de token, refrescar tokens:
vault token renew -i 24h
ultra creds refresh-all

# Si disco lleno, limpiar:
ultra cleanup old-reports --older-than 90d
ultra cleanup old-evidence --older-than 365d

# Prevenir futuros bloqueos:
ultra config set playbook.default_timeout 7200
ultra config set playbook.step_timeout 1800
ultra config set ansible.ssh_timeout 60
```

### Problema: Alto uso de memoria durante inferencia IA causando kills OOM
**Síntomas**: `dmesg` muestra "Killed process 12345 (python) total-vm:..., OOM", los escaneos fallan con "AI analysis failed: out of memory".

**Causas raíz**:
- Demasiados escaneos concurrentes cargando múltiples modelos IA
- Modelo demasiado grande para la RAM disponible
- Fuga de memoria en motor IA
- No hay espacio swap configurado

**Diagnóstico**:
```bash
# 1. Verificar uso de memoria
free -h
ps aux --sort=-%mem | head -10

# Esperado: ultra-ai-engine usando 12GB+, procesos python para escaneo usando 4GB cada uno

# 2. Verificar tamaños de modelos
ls -lh /models/risk-scoring/risk-v2.1/model.pth
# Esperado: ~2.1GB

# 3. Verificar tareas IA concurrentes
ultra ai tasks list --running

# 4. Verificar uso de swap
swapon --show
free -h
```

**Solución**:
```bash
# Limitar inferencias IA concurrentes
ultra config set ai.max_concurrent_inferences 2
ultra config set scan.ai_batch_size 16  # Reducir desde 32

# Añadir espacio swap (emergencia):
sudo fallocate -l 8G /swapfile
sudo chmod 600 /swapfile
sudo mkswap /swapfile
sudo swapon /swapfile
echo '/swapfile none swap sw 0 0' | sudo tee -a /etc/fstab

# Configurar límites de memoria para contenedor IA (si usa Docker):
docker update --memory=16g --memory-swap=20g ultra-ai-engine

# Usar variante de modelo más pequeña:
ultra config set ai.model risk-v2.1-lite  # 512MB vs 2GB

# Reiniciar motor IA con límites de memoria:
sudo systemctl stop ultra-ai-engine
sudo systemctl set Environment="ULIMIT_-MEM=16777216"  # 16GB
sudo systemctl start ultra-ai-engine

# Monitorear memoria después de cambios:
watch -n 5 'free -h && ps aux | grep ultra-ai | grep -v grep'
```

### Problema: Reportes de cumplimiento con datos faltantes o mostrando "N/A" para controles
**Síntomas**: Reporte de cumplimiento tiene muchos "N/A" o "Sin datos" a pesar de escanear todos los objetivos.

**Causas raíz**:
- Agente de recolección no ejecutándose en sistemas objetivo
- Problemas de conectividad SSH
- Plugins de cumplimiento faltantes en escáner
- Permisos denegados para recolección de evidencia
- Desfase de línea de tiempo (escaneo ejecutado fuera de ventana de reporte)

**Diagnóstico**:
```bash
# 1. Verificar estado del agente de cumplimiento en objetivo
ultra compliance agents status --server web01

# 2. Verificar que el escaneo realmente recogió datos de cumplimiento
grep -i "compliance" /var/reports/scan-web01-20240301.json | head

# 3. Verificar mapeo de controles
ultra compliance controls list --framework pci-dss | grep "1.2.2"

# 4. Verificar recolección de evidencia
ls -la /var/lib/ultra/evidence/pci-dss-20240301/web01/

# 5. Verificar logs del agente en servidor objetivo
ssh web01 "journalctl -u ultra-compliance-agent -n 100"

# 6. Validar plugins de escáner
ultra scan plugins list --engine openvas | grep -i "compliance"
```

**Solución**:
```bash
# Desplegar agente de cumplimiento en objetivo
ultra agent deploy --server web01 --type compliance \
  --ssh-key /etc/ultra/ssh/compliance-key \
  --verify

# Disparar recolección de evidencia manualmente:
ultra compliance collect --server web01 --framework pci-dss \
  --controls 1.2.2,2.2.1,4.1.1 \
  --debug

# Si hay problemas SSH, verificar claves:
ssh -i /etc/ultra/ssh/compliance-key web01 "hostname"

# Instalar plugins de cumplimiento faltantes:
ultra scan plugin install openvas --plugin-family "Policy"
ultra scan engines reload openvas

# Forzar re-escaneo con enfoque de cumplimiento:
ultra scan run web01-compliance \
  --target web01 \
  --engine openvas \
  --scan-type full+compliance \
  --compliance pci-dss,cis \
  --plugins-family Policy,Hardening
```

### Problema: La correlación de inteligencia de amenazas devuelve "No matches" para IOCs conocidos maliciosos
**Síntomas**: Enriquecer IOCs conocidos como maliciosos, pero la correlación IA devuelve 0 coincidencias.

**Causas raíz**:
- Feeds de inteligencia de amenazas no actualizados
- Umbral de confianza de modelo IA demasiado alto
- Formato de IOC incompatible
- Ventana de tiempo demasiado estrecha
- Base de datos de amenazas internas vacía

**Diagnóstico**:
```bash
# 1. Verificar frescura de feeds de inteligencia de amenazas
ultra threat-intel feeds status

# Esperado:
# Nombre Feed            Última Actualización   Estado    Registros
# virus-total            hace 5 min            OK        1,234,567
# alien-vault            hace 12 min           OK        876,543
# mandiant               hace 2h              WARNING   (límite de tasa API)

# 2. Verificar formato de IOC en archivo de entrada
head -5 /tmp/iocs.csv
# Debería ser: tipo,valor,fuente

# 3. Verificar caché de correlación IA
ultra ai cache status --model threat-correlation

# 4. Probar búsqueda manual de IOC individual
ultra threat-intel lookup --type ip --value 185.220.101.45 --sources all

# 5. Verificar base de datos de amenazas interna
sudo psql ultra_db -c "SELECT COUNT(*) FROM threat_iocs WHERE confidence >= 0.8;"
```

**Solución**:
```bash
# Actualizar feeds de amenazas
ultra threat-intel feeds update --all --force

# Bajar umbral de confianza
ultra threat-intel enrich /tmp/iocs.csv \
  --sources virus-total,alien-vault \
  --confidence-threshold 0.6  # Por defecto es 0.8

# Extender ventana de tiempo para correlación
ultra threat-intel correlate \
  --enriched-iocs /var/lib/ultra/threat-intel/enriched.json \
  --siem-source splunk \
  --time-range "30d"  # Por defecto podría ser 7d

# Si base de datos interna vacía, importar IOCs históricos:
ultra threat-intel import /var/backups/threat-iocs-2023.json \
  --source internal-archive \
  --confidence 0.9

# Verificar límites de API y facturación de feeds:
curl -H "x-apikey: $(cat /etc/ultra/secrets/virus-total-api-key)" \
  https://www.virustotal.com/api/v3/users/rate_limit
```

### Problema: Remediation falla con "Rollback plan not found" o "Backup invalid"
**Síntomas**: `ultra remediate apply` falla al intentar reversión o cuando se ejecuta pre-check.

**Causas raíz**:
- Auto-backup deshabilitado (`ULTRA_AUTO_BACKUP=false`)
- Almacenamiento de backup lleno o corrupto
- Snapshot LVM/ZFS no disponible
- Problemas de permisos accediendo directorio de backup
- Backup tomado después de iniciar remediación (condición de carrera)

**Diagnóstico**:
```bash
# 1. Verificar directorio de backup
ls -la /backups/ultra/pre-remediate-*
ls -lh /backups/ultra/ | tail -10

# 2. Verificar integridad del backup
ultra backup verify --snapshot /backups/ultra/pre-remediate-20240301-143022

# 3. Verificar snapshots LVM (si usa LVM)
sudo lvs | grep snap
sudo lvs "vg_name/ultra_data-snap-pre-remediate-20240301-143022"

# 4. Verificar snapshots ZFS
zfs list -t snapshot | grep ultra

# 5. Verificar registro de backup de Ultra en base de datos
sudo psql ultra_db -c "SELECT * FROM backups WHERE snapshot='pre-remediate-20240301-143022';"

# 6. Verificar permisos
namei -l /backups/ultra/pre-remediate-20240301-143022
```

**Solución**:
```bash
# Si faltan backups, crear snapshot manual antes de remediación:
# Para LVM:
sudo lvcreate -s -n ultra_data-snap-pre-rem-$(date +%s) /dev/vg/ultra_data

# Para ZFS:
zfs snapshot ultra_data@pre-remediate-$(date +%Y%m%d-%H%M%S)

# Registrar en base de datos Ultra:
ultra backup record create \
  --snapshot ultra_data-snap-pre-rem-$(date +%s) \
  --type lvm \
  --volume ultra_data \
  --pre-remediate \
  --ticket SEC-2024-001

# Si backups corruptos, restaurar desde último conocido bueno:
ultra backup restore --from /backups/ultra/weekly-20240225.tar.gz \
  --target-db ultra_db \
  --target-volume /var/lib/ultra \
  --dry-run

# Habilitar auto-backup en configuración:
ultra config set backup.auto_create true
ultra config set backup.retention_pre_remediate 30

# Verificar permisos de backup:
sudo chown -R ultra:ultra /backups/ultra
sudo chmod 750 /backups/ultra
```

## Comandos de Reversión

### Reversión de remediación completada
```bash
# Reversión completa con verificación y notificaciones
ultra remediate rollback CVE-2024-0001 \
  --snapshot /backups/ultra/pre-remediate-20240301-143022 \
  --verify-services https://healthchecks.io/ultra/xxx,https://internal-health.company.com/ \
  --post-checks "ultra scan run post-remediate-check --engine openvas --target web01,web02" \
  --notify slack:#security,email:secops@company.com \
  --maintenance-window-approved \
  --dry-run  # Eliminar después de validación

# Ejecución en producción:
ultra remediate rollback CVE-2024-0001 \
  --snapshot /backups/ultra/pre-remediate-20240301-143022 \
  --verify-services \
  --post-checks \
  --notify \
  --force  # Solo si la verificación falla pero se necesita reversión

# Salida esperada:
# [ROLLBACK] Iniciando reversión para CVE-2024-0001
# [SNAPSHOT] Encontrado: ultra_data-snap-pre-remediate-20240301-143022 (tamaño: 45GB)
# [RESTORE] Restaurando snapshot LVM... ✓ (5m 23s)
# [VERIFY] Verificación de salud de servicios: web01=200 OK, web02=200 OK ✓
# [POST-CHECK] Ejecutando escaneo de seguridad... ✓ (0 nuevas vulnerabilidades)
# [NOTIFY] Mensaje Slack enviado, email entregado a 12 destinatarios
# [COMPLETE] Reversión exitosa. Sistema estable. Ticket SEC-2024-001 actualizado.
```

### Reversión de escaneo (eliminar reportes y hallazgos)
```bash
# Eliminar artefactos de escaneo y entradas de base de datos
ultra scan rollback scan-20240301-1430 \
  --delete-reports \
  --delete-findings \
  --notify-team secops \
  --audit-reason "Falso positivo - prueba en entorno staging"

# Si el escaneo creó tickets, cerrarlos:
ultra ticket close --from-scan scan-20240301-1430 \
  --reason "Escaneo de prueba - no se requiere acción" \
  --comment "Revertido via ultra scan rollback"

# Salida:
# [ROLLBACK] Eliminando reporte de escaneo: /var/reports/scan-20240301-1430.json (234MB)
# [ROLLBACK] Removiendo 1,247 hallazgos de base de datos
# [TICKET] Cerrando 12 tickets JIRA (SEC-2024-03-01-001 hasta 012)
# [AUDIT] Reversión registrada en log de auditoría (evento: scan_rollback)
```

### Reversión de configuración
```bash
# Ver historial de cambios de configuración
ultra config history --days 30

# Salida:
# Timestamp              Usuario         Cambio
# 2024-03-01 14:30:15   alice          Set: scan.max_concurrent=4
# 2024-03-01 12:15:42   bob            Set: ai.confidence_threshold=0.87
# 2024-02-29 09:22:11   system         Auto-backup creado

# Dry-run de reversión a timestamp específico
ultra config rollback --to "2024-03-01-12:00:00" --dry-run --diff

# Esperado:
# Revertiría 3 cambios:
#   - Revertir ai.confidence_threshold 0.87 → 0.85
#   - Revertir scan.max_concurrent 4 → 2
#   - Restaurar archivo de configuración desde backup: /backups/config/ultra-20240301-121500.yaml

# Realizar reversión actual con validación
ultra config rollback --to "2024-03-01-12:00:00" \
  --validate \
  --test-connections \
  --restart-services \
  --notify-slack \
  --ticket CONFIG-ROLLBACK-2024-001

# Salida:
# [ROLLBACK] Cargando backup: /backups/config/ultra-20240301-121500.yaml
# [VALIDATE] Sintaxis YAML: OK
# [VALIDATE] Esquema: OK
# [TEST] Conexión Vault: OK
# [TEST] Conectividad de escáner: OK
# [REINICIAR] ultra-orchestrator: ✓
# [REINICIAR] ultra-ai-engine: ✓
# [NOTIFY] Mensaje Slack enviado a #ops-alerts
# [TICKET] Creado: CONFIG-ROLLBACK-2024-001
```

### Recuperación de punto en tiempo de base de datos (PITR)
```bash
# Restaurar base de datos a hora específica con reproducción de transacciones
ultra db restore \
  --backup /backups/postgresql/ultra_db-20240301-1200.base.tar.gz \
  --wal-archive /backups/postgresql/wal/ \
  --recovery-target "2024-03-01 14:23:45 UTC" \
  --verify-checksum \
  --maintenance-mode \
  --notify db-team,secops \
  --ticket DB-RESTORE-2024-001

# Recuperación por streaming WAL (continua):
ultra db restore streaming \
  --restore-point /backups/postgresql/full-20240301-0000 \
  --wal-target "2024-03-01 14:30:00" \
  --replication-slot ultra_recovery_slot \
  --standby-mode \
  --switchover

# Salida esperada:
# [RESTORE] Restaurando backup base (45GB)... ✓ (12m 34s)
# [WAL] Reproduciendo archivos WAL desde 2024-03-01 00:00:00 a 2024-03-01 14:23:45
# [WAL] Reproducidos 15,423 registros WAL (2.1GB)
# [RECOVERY] Base de datos recuperada a estado consistente en 2024-03-01 14:23:45 UTC
# [VERIFY] Checksum: OK (sha256: abc123...)
# [MAINTENANCE] Traiendo primary de vuelta en línea...
# [NOTIFY] Email enviado a db-team@company.com
```

### Reversión completa del sistema (emergencia)
```bash
# Reversión completa del sistema a snapshot conocido bueno
# ADVERTENCIA: Esto revierte TODOS los componentes (DB, config, reportes, evidencia)

ultra system rollback \
  --snapshot 2024-03-01-full-system \
  --components ultra-orchestrator,ultra-ai-engine,ultra-db,ultra-config \
  --verify-boot \
  --keep-forensics \
  --maintenance-window "now+2h" \
  --approval required:2 \
  --ticket EMERGENCY-2024-001 \
  --escalate \
  --dry-run

# Después de recibir dos aprobaciones (de CISO y Head of Ops):
ultra system rollback \
  --snapshot 2024-03-01-full-system \
  --components all \
  --verify-boot \
  --preserve-forensics /forensics/rollback-$(date +%Y%m%d-%H%M%S) \
  --force \
  --ticket EMERGENCY-2024-001

# Salida:
# [EMERGENCY] Reversión completa de sistema iniciada (aprobada por: alice, bob)
# [SNAPSHOT] Snapshot completo de sistema: ultra-full-20240301-0000.tar.gz (125GB)
# [COMPONENT] Apagando: ultra-orchestrator, ultra-ai-engine, ultra-db
# [RESTORE] Restaurando PostgreSQL desde backup... ✓
# [RESTORE] Restaurando configuración Ultra desde /backups/config/ultra-20240301-0000.yaml ✓
# [RESTORE] Restaurando modelos IA desde /backups/models/20240301/ ✓
# [BOOT] Iniciando servicios: ultra-db → ultra-ai-engine → ultra-orchestrator
# [VERIFY] Verificación de salud: http://localhost:8080/health → 200 OK
# [VERIFY] Integridad de base de datos: OK
# [PRESERVE] Estado actual guardado en: /forensics/rollback-20240301-1430/
# [NOTIFY] Transmisión de emergencia enviada a all-staff@company.com
# [COMPLETE] Sistema revertido a 2024-03-01 00:00:00 UTC
# [ACCIÓN REQUERIDA] Post-reversión: Reproducir eventos desde 00:00 hasta ahora
#   ultra db replay-wal --start 2024-03-01T00:00:00 --end now
```

### Reversión de inteligencia de amenazas (reversión de auto-bloqueo)
```bash
# Eliminar reglas de firewall creadas por bloqueo de threat-intel
ultra threat-intel rollback \
  --block-id threat-block-20240301-1430 \
  --targets firewall01,proxy01 \
  --verify-removal \
  --notify soc-team \
  --ticket THREAT-ROLLBACK-2024-001

# Si la reversión falla (ej: regla en uso):
ultra threat-intel rollback \
  --block-id threat-block-20240301-1430 \
  --disruptive false \
  --log-only \
  --create-ticket manual-removal-required

# Reversión manual (si auto falla):
# En firewall01:
iptables -L INPUT -n | grep "185.220.101.45"
iptables -D INPUT -s 185.220.101.45 -j DROP
# En proxy:
squidclient -m GET http://proxy01:3128/acl?delete=blacklist_185.220.101.45
```

### Reversión de paso de playbook (durante ejecución)
```bash
# Si un paso de playbook falla y se define reversión automática
ultra playbook step rollback <incident-id> 15 \
  --from-step "deploy-patch" \
  --to-checkpoint "pre-patch-backup" \
  --verify \
  --continue

# Si reversión manual después de completar playbook:
ultra playbook rollback <incident-id> \
  --to-step 10 \
  --snapshot /backups/ultra/playbooks/IR-2024-001-snap-20240301-1430 \
  --notify \
  --ticket IR-ROLLBACK-2024-001

# Esperado:
# [ROLLBACK] Playbook IR-2024-001: Revirtiendo desde paso 20 a paso 10
# [SNAPSHOT] Restaurando /backups/ultra/playbooks/IR-2024-001-snap-20240301-1430
# [PASO] Deshacer paso 20: "rotate-credentials" ✓
# [PASO] Deshacer paso 19: "block-iocs" ✓
# [...]
# [PASO] Deshacer paso 11: "isolate-network" ✓
# [COMPLETE] Sistema retornado a estado después de paso 10 (contención solo, sin remediación)
```

### Reversión de reportes y evidencia
```bash
# Eliminar reporte generado por error y remover evidencia
ultra report rollback \
  --report /reports/pci-dess-exec-20240301.html \
  --evidence-dir /var/lib/ultra/evidence/pci-dss-20240301 \
  --notify compliance@company.com \
  --reason "Plantilla incorrecta usada" \
  --audit

# Salida:
# [ROLLBACK] Eliminando reporte: /reports/pci-dss-exec-20240301.html (4.2MB)
# [ROLLBACK] Removiendo directorio de evidencia: /var/lib/ultra/evidence/pci-dss-20240301/ (1.2GB)
# [AUDIT] Registrado en log de auditoría: report_rollback user=alice reason="Plantilla incorrecta"
# [NOTIFY] Email enviado a compliance@company.com
```

### Reversión de modelo IA (modelo malo causando falsos positivos)
```bash
# Revertir a versión anterior de modelo IA
ultra ai models rollback \
  --model risk-v2 \
  --to-version 2.0.3 \
  --verify \
  --validate-on-samples /var/lib/ultra/test/samples-v2.0.3/ \
  --update-config \
  --notify \
  --ticket AI-MODEL-ROLLBACK-2024-001

# Esperado:
# [ROLLBACK] Modelo de riesgo: v2.1 → v2.0.3
# [LOAD] Cargando modelo anterior desde /models/risk-scoring/v2.0.3/ ✓
# [VALIDATE] Probando en 1,000 vulnerabilidades de muestra...
#   Tasa de falso positivo: 2.1% (objetivo: <5%) ✓
#   Tasa de verdadero positivo: 94.3% (objetivo: >90%) ✓
# [CONFIG] Actualizado: ai.model_version = "2.0.3"
# [NOTIFY] Slack: :warning: Modelo IA revertido a v2.0.3 debido a aumento de FPs
# [TICKET] AI-MODEL-ROLLBACK-2024-001 creado con análisis
```

## Consideraciones de Seguridad

### Control de Acceso y Autenticación
```bash
# Habilitar MFA para todas las operaciones de producción
ultra auth mfa enable --required-for production,remediate,playbook-execute

# Configurar control de acceso basado en roles
ultra role create security-analyst \
  --permissions scan:run,analyze:view,ticket:create \
  --no-remediate,no-config,no-delete

ultra role create secops-admin \
  --permissions all \
  --require-approval-for remediate,playbook:execute,config:set

ultra user assign alice --role security-analyst --expires "2024-12-31"
ultra user assign bob --role secops-admin --mfa-required

# Probar RBAC:
sudo -u alice ultra scan run test  # Debería funcionar
sudo -u alice ultra remediate CVE-1234  # Debería FALLAR: Permiso denegado

# Habilitar grabación de sesión para acciones privilegiadas
ultra session record start --user root --ttl 30d
ultra session replay --session-id sess-20240301-143022
```

### Protección de Datos
```bash
# Cifrar todos los reportes almacenados
ultra config set storage.encryption aes-256-gcm
ultra config set storage.key_vault_path secret/data/ultra/encryption-key

# Habilitar cadena de custodia de evidencia
ultra config set evidence.chain_of_custody true
ultra config set evidence.immutable_storage s3://ultra-evidence/immutable/

# Configurar eliminación segura
ultra config set data_retention.secure_delete_after 3650d
ultra cleanup secure --older-than 3650d --shred passes:3

# Exportar claves de cifrado a HSM (si disponible):
ultra keys export --to hsm://pkcs11:lib/usr/lib/softhsm/libsofthsm2.so \
  --pin-file /etc/ultra/secrets/hsm-pin \
  --key-label ultra-master-key
```

### Seguridad de Red
```bash
# Restringir API a red interna
ultra api bind 10.0.0.0/8 --tls-cert /etc/ultra/ssl/api.pem \
  --tls-key /etc/ultra/ssl/api-key.pem \
  --client-ca /etc/ultra/ssl/ca.pem \
  --require-client-cert

# Configurar límite de tasa de API por usuario
ultra api rate-limit set --user alice --requests-per-minute 60
ultra api rate-limit set --user bob --requests-per-minute 120

# Configurar reglas de firewall
sudo ufw allow from 10.0.0.0/8 to any port 8080,9090 proto tcp
sudo ufw deny 8080,9090

# Habilitar acceso solo VPN (si se requiere):
ultra api bind 100.64.0.0/10  # Rango IP interna WireGuard
```

### Registro y Monitoreo
```bash
# Habilitar registro de auditoría detallado
ultra config set audit.enabled true
ultra config set audit.log_level detailed
ultra config set audit.sign_entries true
ultra config set audit.retention 2555d  # 7 años

# Reenviar logs a SIEM
ultra logging forward --type audit --target splunk://splunk-hec:8088 \
  --token $SPLUNK_HEC_TOKEN \
  --index ultra_audit

ultra logging forward --type operational --target elasticsearch://es01:9200 \
  --index ultra-operations

# Configurar alertas para eventos críticos
ultra alert create "Intento de acceso no autorizado" \
  --condition "ultra_audit_event{event_type='auth_failure'} > 0" \
  --severity critical \
  --runbook "https://runbooks.company.com/unauth-access" \
  --notify pagerduty,slack:#security-alerts \
  --for 5m
```

## Ajuste de Rendimiento

### Optimización de Escaneo
```bash
# Ajustar configuración de escaneo paralelo basada en recursos disponibles
ultra config set scan.max_concurrent_jobs 4          # Por defecto: 2
ultra config set scan.max_hosts_per_job 50         # Por defecto: 25
ultra config set scan.max_ports_per_host 100       # Por defecto: 50
ultra config set scan.rate_limit_global 100        #req/min global

# Límites de tasa por motor (para no saturar objetivos)
ultra config set scan.rate_limits.openvas 10       #req/min
ultra config set scan.rate_limits.nessus 20        #req/min

# Procesamiento por lotes IA (reducir huella de memoria)
ultra config set ai.inference_batch_size 16         # Por defecto: 32
ultra config set ai.model_cache_size 2048          # Entradas de caché

# Pool de conexiones de base de datos
ultra config set db.pool_size 20                   # Por defecto: 10
ultra config set db.max_overflow 30                # Conexiones temporales

# Caché Redis para consultas frecuentes (opcional)
ultra config set cache.redis_url redis://redis01:6379/0
ultra config set cache.ttl_scan_results 3600      # 1 hora
ultra config set cache.ttl_ai_inference 1800      # 30 minutos
```

### Gestión de Memoria
```bash
# Establecer límites de memoria por componente (systemd)
sudo systemctl edit ultra-orchestrator

# Añadir:
[Service]
MemoryLimit=8G
MemorySwapMax=12G

# Para motor IA (consumidor de memoria):
sudo systemctl edit ultra-ai-engine
[Service]
MemoryMax=16G
IOWeight=100  # Priorizar IO

# Configurar ULTRA para respetar límites del sistema
ultra config set resources.memory_limit 24G
ultra config set resources.soft_limit 20G
ultra config set resources.oom_score_adj -500  # Menos probabilidad de ser matado

# Habilitar swap como red de seguridad
sudo fallocate -l 8G /swapfile
sudo chmod 600 /swapfile
sudo mkswap /swapfile
sudo swapon -p 10 /swapfile  # Prioridad 10

# Monitorear presión de memoria
ultra metrics watch memory_usage --alert-threshold 85%
```

### Rendimiento de Base de Datos
```bash
# Ajustar PostgreSQL para carga de trabajo Ultra
sudo nano /etc/postgresql/14/main/ultra.conf

# Añadir:
shared_buffers = 4GB
effective_cache_size = 12GB
work_mem = 64MB
maintenance_work_mem = 1GB
max_connections = 100
max_parallel_workers_per_gather = 4

# Crear índices para consultas comunes
sudo -u postgres psql ultra_db -c "
CREATE INDEX CONCURRENTLY idx_scans_completed ON scans (completed_at) WHERE status='completed';
CREATE INDEX CONCURRENTLY idx_vulnerabilities_cvss ON vulnerabilities (cvss_score DESC);
CREATE INDEX CONCURRENTLY idx_incidents_severity ON incidents (severity, created_at);
"

# Mantenimiento regular vacuum y analyze
ultra db maintenance --auto-vacuum --analyze --daily 02:00

# Particionar tablas grandes por fecha
ultra db partition scans --by month --retention 90d
```

### Optimización de Red
```bash
# Usar pool de conexiones para APIs externas
ultra config set http.connection_pool_size 50
ultra config set http.keepalive_timeout 60
ultra config set http.max_retries 3
ultra config set http.backoff_factor 0.5

# Configurar descarga paralela de resultados de escaneo
ultra config set scan.download_concurrency 10

# Usar caché local para feeds de inteligencia de amenazas
ultra config set threat_intel.cache_ttl 3600  # 1 hora
ultra config set threat_intel.cache_size 10000  # entradas
```

## Mantenimiento

### Chequeos Diarios de Salud (Automáticos)
```bash
#!/bin/bash
# /opt/ultra/scripts/healthcheck.sh

/usr/local/bin/ultra status --detailed > /var/log/ultra/healthcheck-$(date +%Y%m%d).log

# Verificar servicios críticos
if ! systemctl is-active --quiet ultra-orchestrator; then
  echo "CRITICAL: servicio ultra-orchestrator caído" | mail -s "Alerta Ultra" secops@company.com
fi

# Verificación de espacio en disco
df -h /var/reports | awk '$5+0 > 90 {print "ADVERTENCIA: Partición de reportes >90% llena"}'

# Verificación de conexión de base de datos
/usr/local/bin/ultra db ping || echo "CRITICAL: Base de datos inalcanzable"

# Verificación de rotación de logs
if [ $(find /var/log/ultra -name "*.log" -size +100M | wc -l) -gt 0 ]; then
  echo "ADVERTENCIA: Archivos de log grandes detectados" | mail -s "Alerta de Logs Ultra" secops@company.com
fi

# Frescura de modelos IA
ultra ai models list | grep -i "expired" && echo "ADVERTENCIA: Modelo IA expirado"

# Añadir a cron: 0 6 * * * /opt/ultra/scripts/healthcheck.sh
```

### Tareas de Mantenimiento Semanales
```bash
#!/bin/bash
# /opt/ultra/scripts/weekly-maintenance.sh

# 1. Limpiar reportes antiguos (mantener 90 días)
ultra cleanup old-reports --older-than 90d --dry-run
ultra cleanup old-reports --older-than 90d

# 2. Archivar evidencia antigua a S3 (mantener 1 año local, 7 años S3)
ultra evidence archive --older-than 365d --to s3://ultra-evidence/long-term/ --dry-run
ultra evidence archive --older-than 365d --to s3://ultra-evidence/long-term/

# 3. Actualizar modelos IA (desde staging después de validación)
ultra ai models update --channel staging --dry-run
# [Humano] Validar modelos staging en datos de muestra
ultra ai models update --channel production

# 4. Rotar logs y archivos de auditoría
sudo logrotate -f /etc/logrotate.d/ultra-orchestrator
ultra audit compress --older-than 90d

# 5. Mantenimiento de base de datos
ultra db vacuum --full --analyze
ultra db reindex --concurrently

# 6. Verificar y notificar sobre escaneos/incidentes fallidos
ultra scans list --status failed --last-week | mail -s "Escaneos Fallidos Semanales" secops@company.com
ultra incidents list --status open --older-than 7d | mail -s "Incidentes Obsoletos" secops@company.com

# Añadir a cron semanal: 0 2 * * 0 /opt/ultra/scripts/weekly-maintenance.sh
```

### Tareas Mensuales
```bash
#!/bin/bash
# /opt/ultra/scripts/monthly-maintenance.sh

# 1. Backup completo del sistema
ultra backup create --type full --compression gz --verify

# 2. Auditoría de seguridad
ultra audit compliance --check all --output html --email compliance@company.com

# 3. Revisión RBAC
ultra role list --inactive-users > /tmp/inactive-users.txt
ultra user review --inactive-file /tmp/inactive-users.txt --disable

# 4. Actualizaciones de dependencias de terceros
ultra dependencies check-updates
# Revisar y aprobar actualizaciones en staging primero

# 5. Reporte de planificación de capacidad
ultra metrics report --period 30d --output /reports/capacity-$(date +%Y%m).pdf

# 6. Validación de licencias y suscripciones
ultra licenses check --all --notify finance@company.com

# Añadir a cron mensual: 0 3 1 * * /opt/ultra/scripts/monthly-maintenance.sh
```

### Tareas Trimestrales
```bash
#!/bin/bash
# /opt/ultra/scripts/quarterly-maintenance.sh

# 1. Ejercicio de recuperación de desastre
ultra disaster-recovery drill \
  --scenario full-outage \
  --components db,config,models \
  --target-backup /backups/ultra/quarterly/$(date +%Y%m) \
  --verify-restore \
  --document-results

# 2. Coordinación de prueba de penetración (proporcionar evidencia a pentesters)
ultra pentest provide-evidence --targets all-prod \
  --evidence-dir /var/lib/ultra/evidence/pentest-Q$(date +%q)-$(date +%Y) \
  --read-only-access \
  --ttl 30d

# 3. Validación de reentrenamiento de modelos IA
ultra ai models validate-training-data \
  --period 90d \
  --feedback-from incident-reports,analyst-ratings \
  --report /reports/ai-model-validation-Q$(date +%q).pdf

# 4. Auditoría de seguridad de configuración
ultra config audit --check security --output html --fix-drift

# 5. Actualizar playbooks basados en lecciones aprendidas de incidentes
ultra playbook update-from-incidents --last-quarter

# 6. Renovación de evaluación de seguridad de proveedores
ultra vendors security-review --all --document /reports/vendor-security-Q$(date +%q).pdf

# Trimestral: Ejecutar manualmente después de coordinación con equipo de seguridad
```

### Procedimientos de Emergencia

#### Fallo Completo del Sistema
```bash
# 1. Evaluar alcance
ultra status --emergency
# Si API caída, verificar via SSH:
sudo systemctl status ultra-*

# 2. Verificar logs
sudo journalctl -u ultra-orchestrator -n 1000 --no-pager
sudo tail -100 /var/log/ultra/ultra.log

# 3. Reiniciar en orden
sudo systemctl restart postgresql
sudo systemctl restart ultra-db
sudo systemctl restart ultra-ai-engine
sudo systemctl restart ultra-orchestrator

# 4. Si sigue fallando, restaurar desde backup
ultra system restore --backup /backups/ultra/full-20240301-0000.tar.gz \
  --verify \
  --emergency

# 5. Si se sospecha pérdida de datos, contactar soporte con:
# - salida de ultra doctor --full --json
# - /var/log/ultra/audit.log (últimas 10,000 líneas)
# - Volcados de base de datos: sudo pg_dump ultra_db > ultra_db-dump-$(date).sql
```

#### Incidente de Seguridad que Involucra a Ultra
```bash
# Si el orquestador Ultra mismo comprometido:
# 1. Aislar inmediatamente
sudo iptables -I INPUT -p tcp --dport 8080 -j DROP
sudo iptables -I INPUT -p tcp --dport 9090 -j DROP

# 2. Preservar evidencia
sudo cp -a /var/log/ultra /forensics/ultra-logs-$(date +%Y%m%d-%H%M%S)
sudo tar czf /forensics/ultra-filesystem-$(date +%Y%m%d-%H%M%S).tar.gz /opt/ultra

# 3. Capturar memoria
sudo dd if=/dev/mem of=/forensics/ultra-memory-$(date +%Y%m%d-%H%M%S).raw bs=1M

# 4. Revocar todas las credenciales
ultra credentials revoke-all --emergency --notify secops

# 5. Restaurar desde backup conocido bueno
ultra system restore --from-backup /backups/ultra/full-trusted.tar.gz \
  --clean-install \
  --rotate-all-keys \
  --notify

# 6. Análisis forense post-incidente
ultra incident create --type security --title "Compromiso de Ultra Orchestrator" \
  --evidence-dir /forensics/ultra-* \
  --forensic-analysis required
```

## Cumplimiento de Políticas

### Política de Retención de Datos
```bash
# Configurar programas de retención inmutables
ultra config set retention.audit_logs 2555d      # 7 años (SOX)
ultra config set retention.incident_data 3650d   # 10 años (PCI-DSS)
ultra config set retention.vulnerability_data 1095d  # 3 años
ultra config set retention.reports 2555d

# Forzar política WORM (Write Once Read Many) para datos de cumplimiento
ultra storage set-policy compliance-evidence \
  --worm-enabled \
  --retention 3650d \
  --path /var/lib/ultra/evidence/

# Verificación automatizada de limpieza
ultra cleanup verify --dry-run --report /reports/cleanup-plan-$(date +%Y%m).pdf
```

### GDPR y Privacidad
```bash
# Derecho al olvido - eliminación de datos
ultra privacy delete-user --user-id johndoe@example.com \
  --from-logs \
  --from-reports \
  --from-evidence \
  --anonymize-analytics \
  --generate-certificate

# Exportación de datos para solicitudes GDPR
ultra privacy export-user --user-id johndoe@example.com \
  --format json,pdf \
  --include scans,incidents,reports \
  --deliver-to secure-ftp://gdpr-exports/

# Reporte de evaluación de impacto en privacidad
ultra privacy generate-pia --period 2024 --output /reports/PIA-2024.pdf
```

### Controles ISO 27001
```bash
# Generar paquete de evidencia ISO 27001
ultra compliance evidence collect \
  --framework iso-27001 \
  --controls A.9.1,A.9.2,A.12.2,A.12.4,A.16.1 \
  --auditor "KPMG" \
  --period 2024-01-01:2024-03-31 \
  --output /evidence/iso27001-A1-Q1-2024.zip \
  --signed-by cisoc@company.com
```

---