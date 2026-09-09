# Inventario de etiquetas de precio

## Recomendación de repositorios

Usa DOS repositorios:

1. Repositorio WEB (puede ser público): contiene `index.html`, `manifest.json`, `sw.js` y esta documentación. Activa GitHub Pages.
2. Repositorio DATOS (PRIVADO): almacena el histórico y el stock teórico en `data/inventario-etiquetas.json`.

No pongas el token ni los Excel en el repositorio público.

## Permisos del token

Crea un Fine-grained Personal Access Token limitado únicamente al repositorio privado de datos y con permiso de lectura/escritura sobre Contents. Pega el token en la aplicación al iniciar la sesión. La aplicación no lo escribe en el JSON ni en el repositorio.

## Primera puesta en marcha

1. Abre la web.
2. Ve a "Nube e histórico" y conecta el repositorio privado.
3. En "Cálculo", carga Stock físico, Maestro, Tecleado y Salidas.
4. Marca "El stock cargado es un nuevo recuento físico".
5. Calcula y pulsa "Guardar corte y actualizar stock".

A partir de ahí el stock físico puede omitirse en semanas normales: el stock teórico se mantiene restando el incremento de Salidas y sumando pedidos cuando se marcan como recibidos.

## Fórmula recomendada

Demanda de etiquetas = MAX(Tecleado - Salidas, 0) x porcentaje de reetiquetado.

Seguridad inicial = demanda x (días de seguridad / 15). Por defecto se usan 3 días = 20%.

Desde 4 cortes históricos, la app calcula también una seguridad estadística con la desviación estándar del consumo semanal y el nivel de servicio elegido. Usa el mayor entre la cobertura mínima y la seguridad estadística.

Pedido = redondeo al múltiplo de MAX(demanda + seguridad - stock teórico - pedidos pendientes, 0).

## Histórico de pedidos

- "Confirmar pedido recomendado" guarda el pedido como PENDIENTE y lo suma a "En camino".
- Un pedido externo puede importarse desde la pestaña Pedidos.
- Cuando llegue físicamente, pulsa "Marcar recibido". Sus unidades pasan al stock teórico.
- Los pedidos anulados permanecen en histórico pero dejan de contar como pendientes.

## Privacidad

Los Excel se leen en el navegador. El histórico derivado se guarda en el repositorio privado que configures. La librería SheetJS se carga desde cdnjs para leer/escribir Excel; si necesitas una instalación sin dependencias externas, descarga `xlsx.full.min.js` versión 0.18.5 y cambia el `<script src=...>` de `index.html` por `./xlsx.full.min.js`.
