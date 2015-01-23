## WS-Addressing

### Allgemein
[WS-Addressing](http://de.wikipedia.org/wiki/WS-Addressing) muss benutzt werden, wenn der Consumer nicht direkt mit dem Provider spricht, wenn es also Zwischenstationen gibt (==AXA: Mediator?==)

### Aktivierung
Annotation `@Addressing` in der `...ServiceImpl`.
``` Java
@WebService(...)
@Addressing(enabled=true, required=true)
public class WebserviceImpl {
...
```
