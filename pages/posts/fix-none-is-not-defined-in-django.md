---
title: Fix "None Type is not iterable " in Django
description: Slides & transcript for my talk at VueDay 2021
date: 2022-07-19T16:00:00.000+00:00
lang: en
recording: false
duration: 30min
---
## Error Message
![image-20220614174318585](https://image-1256777099.cos.ap-beijing-fsi.myqcloud.com/2022/image-20220614174318585.png)

According to the breakpoint debug, I found it's caused by the context processors that I had defined.

Comment it first. 

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
Comment this line, the application turned into a normal state.

It seems that the context processor changed the context that login page needed. 

We are in an **unauthorized** state when we enter the system for the first time. So we need to handle this situation.

## Solution

The right way to declare a context processor is below:
```python
### food/context_processors.py
from .models import OrderItem, Order


# the order item number of you current cart
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

