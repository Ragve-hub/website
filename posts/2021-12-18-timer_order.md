---
title:  "Отмена ордера по таймеру"
date:   2021-12-18
categories: [C#]
---


На примере показана отмена лимитного ордера через 20 секунд, если он не not filled.


## Код

```c#
using OpenQuant.API;

public class MyStrategy : Strategy
{
   private Order order;
   private bool entry = true;
   
   public override void OnBar(Bar bar)
   {
      if (HasPosition)
         ClosePosition();

      if (entry)
       {
         order = LimitOrder(OrderSide.Buy, 100, bar.Close - 0.03);                     
         order.Send();
            
         AddTimer(Clock.Now.AddSeconds(20));

         entry = false;
      }
   }
   
   public override void OnTimer(DateTime signalTime)
   {   
      if (!order.IsDone)
         order.Cancel();

      entry = true;
   }
}

```
