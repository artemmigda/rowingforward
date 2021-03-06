---
title: О мелочах, которые бесят [2]
---

В 3.10 стало удобно записывать кастрированные питоновские типы-суммы в аннотациях. Вместо `Union[A, B]` теперь можно на хаскельный манер писать `A | B`. И вместе с этой фичей появилась отвратительная, как на мой вкус, неоднозначность. 

Достаточно просто попробовать переопределить `__or__` в метаклассе класса `A` и значение выражения `A | B`, очевидно, изменится, в том числе и в аннотациях. Причем, интерпретатор на пару с линтером будут молчать, вам никто не запретит это сделать, вам никто не ругнётся, что то, что вы только что зааннотировали это не `Union` — выражение просто вычислится, а его результат интерпретатор воспримет как тип для аннотации. Вот так:

```python
class A:
    pass

class B:
    pass

class MetaC(type):
    def __or__(self, other):
        return A

class C(metaclass=MetaC):
    pass

some_type: C | B  # annotated as A
```

Вы, наверное, думаете, что никому в голову не придёт наследовать `__or__` в метаклассе? Ну, например, мне пришло. А зачем оно мне понадобилось я расскажу как-нибудь в другой раз.