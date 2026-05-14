Генерируем словарь из достаточно большого корпуса русскоязычного текста (тут "Война и Мир" + "Преступление и наказание" + "Тихий Дон")
```
> python hest --mkdict in.txt out.json

Initial Cyrillic words extracted: 1050674
Stage 1: Register normalization...
After register normalization: 1050674 words
Stage 2: Sorting dictionary...
After sorting: 1050674 words
Stage 3: Deduplication...
After deduplication: 115263 unique words
Stage 4: Isolation of proper names...
After proper name isolation: 105319 words
Stage 5: Checking minimum dictionary size...
Dictionary size check passed: 105319 words
Stage 6: Iterative thinning of dictionary to exactly 65792 words...
Iteration 1: current size = 105,319, deletion step = 3
  -> after iteration 1: 70,212 words
Iteration 2: current size = 70,212, deletion step = 16
  -> after iteration 2: 65,823 words
Iteration 3: current size = 65,823, deletion step = 2124
  -> after iteration 3: 65,792 words
Stage 6 completed successfully. Final dictionary size: 65792
Stage 7: Forming 8-bit dictionary...
Extracted 256 words for 8-bit dictionary
Main 16-bit dictionary size after extraction: 65536 words
Stage 8: Forming final dictionary JSON...
Calculating dictionary SHA-256 checksum...
Calculating punctuation probabilities...
Writing dictionary to 'out.json'...
Dictionary successfully written to 'out.json' (256 8-bit + 65536 16-bit words)
```

Словари должны быть одинаковы на приёмнике и передатчике, для контроля в структуре словаря предусмотрен хеш (см. `out.json`)

Генерируем тестовый пакет информации
```
> python randgen 256 > test.dat
> openssl md5 test.dat
MD5(test.dat)= 763b96dd75ed2e0e25a2e17f885f6fd8
```

Кодируем через Herzen-Street и смотрим результат.
```
> type test.dat | python hest -q -e --dict ./out.json > encoded.txt
> python printfile encoded.txt
Смолкли. Семейство, Сущевской следовал названием дерутся белый, запененном дремлете, зажиточно.
Перекрестки восстановляющую заторе рядами обметана связных. Платки принуждала шепелявя, вмешиваясь
долетели пастуха, мучайся тенью коноплю Конькову прибегает. Следующую, родственником повозка мясным
обувь девичьи тащут, нападал распаленный, увертливые, привязана, дребезжаньем кормильцы предстательству
страдающим чудовищности, алексеевского выпаривая прищурились Кобылятниковым, порядочные пихаю беспутное
единомышленников Остри сшиблись взаправду отсвета расплакалась хлопаньем вытоптанными, упирая, Пруссия
пресспапье облупившимся подвальном известью суете, плюну рано обжиться. Ощущаемого Отклонив родная Волгой
Коловейдина оповещенное приказного отрезаны согнутый, попадаться. Спасибо завистливые гадок, чучеле,
роняли — преданную храбреца отслужил полками нестерпимый, решуся бледненькое, судьи повременили милях
пристрельный, Федоре. Сообщая представлений объезженного мышиную слесарь сезон Феоктист полуфразе,
отношение зажигайт витая знанием купив, поездим отложенного, слетевшую росинками, своенравной ища классной,
старичишка — Платовским Родька смущенного признание зверства молимся. Обведенного угадала, отвергнув
коз провалившимся мудрое
```

(Здесь вывод поправлен руками - добавил word wrap для читаемости. Реальный кодер не генерирует newline)

Декодируем и проверяем совпадение
```
> type encoded.txt | python hest -q -d --dict ./out.json > test_dec.dat
> openssl md5 test.dat test_dec.dat
MD5(test.dat)= 763b96dd75ed2e0e25a2e17f885f6fd8
MD5(test_dec.dat)= 763b96dd75ed2e0e25a2e17f885f6fd8
```
