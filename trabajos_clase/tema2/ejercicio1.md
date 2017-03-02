# Ejercicio 1
## Calcular la disponibilidad del sistema descrito en [edgeblog.net](http://www.edgeblog.net/2007/in-search-of-five-9s/ "edgeblog.net") si en cada subsistema tenemos en total 3 elementos.
Los datos que nos ofrecen son los siguientes:

| Componente       | Disponibilidad |
| ---------------  | -------------- |
| Web              | 85%            |
| Aplicación       | 90%            |
| Base de datos    | 99.9%          |
| DNS              | 98%            |
| Firewall         | 85%            |
| Switch           | 99%            |
| Centro de datos  | 99.99%         |
| ISP              | 95%            |

Usando la fórmula *As = Ac1 + ((1 – Ac1) * Ac2)*, obtenemos las disponibilidades asociadas a cada componente si los duplicásemos dos veces:

| Componente       | Disponibilidad    |
| ---------------  | ----------------- |
| Web              | 97.75%            |
| Aplicación       | 99%               |
| Base de datos    | 99.9999%          |
| DNS              | 99.96%            |
| Firewall         | 97.75%            |
| Switch           | 99.99%            |
| Centro de datos  | 99.99%            |
| ISP              | 99.75%            |

Usando la misma fórmula recursivamente, es decir *As = Ac(n-1) + ((1 – Ac(n-1)) * Acn)*, obtendremos las disponibilidades asociadas a cada componente si los 3 veces: 

| Componente       | Disponibilidad      |
| ---------------  | ------------------- |
| Web              | 99.6625%            |
| Aplicación       | 99.9%               |
| Base de datos    | 99.9999999%         |
| DNS              | 99.9992%            |
| Firewall         | 99.6625%            |
| Switch           | 99.9999%            |
| Centro de datos  | 99.99%              |
| ISP              | 99.9875%            |

Por tanto, para calcular la disponibilidad total del sistema usamos *As = Ac1 * Ac2 * Ac3 * ... Acn* y será:
**As = 99.2%**