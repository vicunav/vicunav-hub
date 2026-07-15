# ADR 0001: Separación entre theme y plugins

## Contexto

Se estableció que un theme debía sobrevivir a un cambio de diseño sin perder lógica de
negocio y que un plugin debía funcionar sin depender de un theme específico. La
separación de estas responsabilidades evitaba que la presentación condicionara el
comportamiento funcional del ecosistema.

## Decisión

Se decidió que `theme-core` contendría únicamente elementos de presentación: patrones,
tokens y templates. Toda la lógica de negocio residiría en plugins.

## Consecuencias

Se consideró una violación de esta decisión cualquier CPT, regla de negocio o dato
transaccional que apareciera en el theme. Los cambios de diseño podrían reemplazar el
theme sin afectar la lógica de negocio, y los plugins podrían funcionar sin depender de
un theme específico.
