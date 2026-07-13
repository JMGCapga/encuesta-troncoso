# Encuesta de Satisfacción - Materiales Troncoso

Página de encuesta digital para recopilar feedback de clientes después de la entrega.

## Características

- ✅ 5 preguntas de satisfacción (Sí/No)
- 📝 Campo de comentarios opcional
- 📱 Optimizado para móvil (botones grandes para usar con guantes)
- 🎁 Incentivo: playera de regalo por reseña en Google Maps
- 🔗 Sin login requerido — acceso por link directo
- 🔥 Integración con Firebase Firestore

## Uso

Link con parámetros:
```
https://jmgcapga.github.io/encuesta-troncoso?pedido=TRO-045&cliente=Juan%20García
```

Parámetros:
- `pedido`: ID del pedido (ej. TRO-045)
- `cliente`: Nombre del cliente

## Diseño

- Color naranja: `#E8500A` (Materiales Troncoso)
- Botones SÍ: Verde `#22C55E`
- Botones NO: Gris `#F0EDE8`
- Fuentes: Bebas Neue (títulos), Barlow (texto)
- Mobile-first responsive

## Firebase

- Proyecto: `troncoso--sistema`
- Colección: `encuestas`
- Estructura de documento:
  ```json
  {
    "pedidoId": "TRO-045",
    "cliente": "Juan García",
    "fecha": "2026-07-13",
    "timestamp": 1689270000000,
    "llegóATiempo": true,
    "productosCorrectos": true,
    "cantidadesCorrectas": true,
    "descargaSinDanos": true,
    "atencionBuena": true,
    "comentarios": "Todo excelente",
    "puntuacion": 5,
    "dejóResena": false
  }
  ```

## Deploy

1. Crear repo en GitHub: `JMGCapga/encuesta-troncoso`
2. Push a main
3. Activar GitHub Pages en Settings → Pages → main
4. La página estará disponible en: `https://jmgcapga.github.io/encuesta-troncoso`

---
Materiales Troncoso © 2026
