---
title: 修复Django中出现的"None Type is not iterable"错误 
description: Slides & transcript for my talk at VueDay 2021
date: 2022-07-19T16:00:00.000+00:00
lang: zh
recording: false
duration: 30min
---
## Error Message
![image-20220614174318585](https://image-1256777099.cos.ap-beijing-fsi.myqcloud.com/2022/image-20220614174318585.png)

根据断点调试，我发现它是由我定义的上下文处理器引起的。

先注释一下。

```python
# settings.py
TEMPLATES = [
    {
        'BACKEND': 'django.template.backends.django.DjangoTemplates',
        'DIRS': [BASE_DIR / 'templates', os.path.join(BASE_DIR, 'templates')]
        ,
        'APP_DIRS': True,
        'OPTIONS': {
            'context_processors': [
                'django.template.context_processors.debug',
                'django.template.context_processors.request',
                'django.contrib.auth.context_processors.auth',
                'django.contrib.messages.context_processors.messages',
                # 'food.context_processors.access_realtime_cart_count'
            ],
        },
    },
]
```

## Reason
注释这一行，应用程序变成了正常状态。

似乎上下文处理器更改了登录页面所需的上下文。

当我们第一次进入系统时，我们处于**未授权**状态。 所以我们需要处理这种情况。

## Solution

声明上下文处理器的正确方法如下：
```python
### food/context_processors.py
from .models import OrderItem, Order
```

您当前购物车的订单商品编号

```py
def access_realtime_cart_count(request):
    if request.user.is_authenticated:
        try:
            active_order = Order.objects.get(user__username=request.user.username, status=1)
        except Order.DoesNotExist:
            newOrder = Order(user=request.user, total_price=0.0, status=1)
            newOrder.save()
            active_order = newOrder
        cart_count = OrderItem.objects.filter(order_id=active_order.id).count()
        return {'realtimeCartCount': cart_count} # you can access the "cart_count" variable in any tempaltes of your projec
    else: # handle the unlogin 
        return {'realtimeCartCount': 0}
```

