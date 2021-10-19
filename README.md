# sinirsiz-ac-kapat-pot-sebnem

game/src/char_item.cpp

>Arat:
```
case USE_SPECIAL:
```
>Altına Ekle:
```
#ifdef ENABLE_UNLIMITED_POTIONS
						if (item->GetValue(3) == 4248)
						{
							DWORD dwType = item->GetValue(0);
							BYTE bBonus = aApplyInfo[item->GetValue(1)].bPointType;

							if (AFFECT_NONE == dwType)
								break;

							CAffect * pAffect = FindAffect(dwType, bBonus);

							if (!pAffect)
							{
								DWORD dwFlag = item->GetValue(4);
								AddAffect(dwType, bBonus, item->GetValue(2), dwFlag, INFINITE_AFFECT_DURATION, 0, true, true);

								item->Lock(true);
								item->SetSocket(0, true);
							}
							else
							{
								if (item->GetSocket(0))
								{
									RemoveAffect(pAffect);
									item->Lock(false);
									item->SetSocket(0, false);
								}
							}
							break;
						}
#endif
```

common/service.h

>Ekle:
```
#define ENABLE_UNLIMITED_POTIONS
```

root/uiinventory.py

>Arat:
```
	def RefreshBagSlotWindow(self):
```
>Bul:
```
		self.wndItem.RefreshSlot()
```
>Üstüne Ekle:
```
			if app.ENABLE_UNLIMITED_POTIONS:
				item.SelectItem(itemVnum)
				if item.GetValue(3) == 4248 or item.GetValue(3) == 42480:
					metinSocket = [player.GetItemMetinSocket(slotNumber, j) for j in xrange(player.METIN_SOCKET_MAX_NUM)]
					isActivated = 0 != metinSocket[0]
					if isActivated:
						self.wndItem.ActivateSlot(slotNumber)
					else:
						self.wndItem.DeactivateSlot(slotNumber)
```

Userinterface/Locale_inc.h

>Ekle:
```
#define ENABLE_UNLIMITED_POTIONS
```
Userinterface/PythonApplicationModule.cpp

>Ekle:
```
#ifdef ENABLE_UNLIMITED_POTIONS
	PyModule_AddIntConstant(poModule, "ENABLE_UNLIMITED_POTIONS", 1);
#else
	PyModule_AddIntConstant(poModule, "ENABLE_UNLIMITED_POTIONS", 0);
#endif
```

```
71027	용신의 생명	ITEM_USE	USE_AFFECT	1	ANTI_DROP | ANTI_SELL | ANTI_GIVE | ANTI_STACK | ANTI_MYSHOP	ITEM_STACKABLE | LOG	NONE		0	0	0	0	0	LIMIT_NONE	0	LIMIT_NONE	0	APPLY_NONE	0	APPLY_NONE	0	APPLY_NONE	0	510	69	20	1800	0	0	0	0	0
```
>USE_AFFECT kısmını USE_SPECIAL olarak değiştir.

>1800 kısmını(Value3) 4248 olarak değiştir.
