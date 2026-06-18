Alcance del módulo: payload SII-IGIC para la ATC, validaciones locales y libros
registro, sin parchear ``l10n_es_aeat_sii_oca`` salvo acuerdo explícito OCA.

**Entregado en este módulo (PR actual)**

- Mapa SII ATC priorizado, transformación IVA→IGIC y parche SFRBI
  (``BienInversion`` en compras de inversión).
- Conexión cautela/producción, ``IDVersionSii`` 1.0/1.1, IDType 02→04.
- Validaciones pre-envío F3: errores 1295, 1349, 2042; régimen 07; F2 > 3.000 €.
- Rectificativa por sustitución (tipo **S**) e ``ImporteRectificacion``.
- Causa de exención **E8** en producto.
- Batería de tests de payload y tests unitarios; casos aplazados con
  ``skipTest`` hasta implementar cada fase.

**Fase 1 — Datos IGIC y mapa (aplazada)**

- Dependencia ``l10n_es_igic`` y mapa ``igic_r_1`` (tipo 1 % petróleo).
- Motivo: alinear con PR IGIC 16.0 antes de duplicar en 18.0.
- Test pendiente: ``test_sale_igic_1_percent_petroleum``.

**Fase 2 — Nodos de payload en facturas (aplazada)**

- Régimen 06 (``BaseImponibleACoste``), 14 (obra AAPP), F3 sustituidas,
  REPEP claves 15/16/18, DUA/``CuotaAIEM``, impuestos especiales, BI ampliado.
- Motivo: port a 18.0 del bloque DUA/impuestos de
  [OCA/l10n-spain#5050](https://github.com/OCA/l10n-spain/pull/5050).
- Tests pendientes: ``test_sale_regime_06_*``, ``test_sale_f3_*``,
  ``test_purchase_repep_*``, ``test_purchase_dua_aiem_cuota``, etc.

**Fase 3 — Validaciones pre-envío — hecho**

- F2 > 3.000 €, régimen 07 vs ISP/exenciones, BienInversion vs 08/18,
  coherencia ``ImporteTotal``, bloqueo local 1295 (E2/E3 con clave 01).

**Fase 4 — Rectificativas ATC — hecho**

- ``TipoRectificativa = S`` y ``ImporteRectificacion`` (solo inherit ATC).

**Fase 5 — RECC criterio de caja (aplazada)**

- Cobros/pagos régimen 07 (``SiiFactCOBV1SOAP`` / ``SiiFactPAGV1SOAP``).
- Motivo: mismo bloque que Fase 2.

**Fase 6 — Libro anual de bienes de inversión (aplazada)**

- Periodo ``0A``, prorrata y regularización; requiere modelo dedicado.

**Fuera de este módulo**

- Parches al SII peninsular genérico → ``l10n_es_aeat_sii_oca`` (PR upstream).
- Impuestos especiales sin equivalente Odoo (tabaco/combustibles): follow-up.

**Orden recomendado de implementación**

Fase 3 → Fase 4 (este PR) → Fase 2 + DUA (#5050) → Fase 5 → Fase 1 → Fase 6.
