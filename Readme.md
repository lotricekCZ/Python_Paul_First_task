# Anotace
Tento repozitář vznikl ve snaze zasvětit bývalého spolužáka do problematiky programování
# **Úkol: Přinesená věc**

Do rukou se ti dostal relativně neznámý předmět, který periodický bliká. Nemusíš okamžitě přestřihávat červený drátek, žádný tam snad ani není, ale nebylo by špatné použít nějaké utilitky na zjištění, co z toho chodí.
Moc jich není, ale pokud máš okno s PyCharmem, hodilo by se pár věcí, z nichž [pyserial](https://pypi.org/project/pyserial/) hraje prim.

Pro základní diagnostiku stačí udělat infinite loop, asi nějak takhle:

```python
"""... nějaký kód tady nahoře"""
while True:
    if s.in_waiting:
	    print(s.readall())
        time.sleep(0.2)
```

Při prvotní analýze lze objevit určitá periodičnost, určité znaky se opakují v první řadě je ale nutno podotknout, že *\xXY*  nejsou 4 znaky, ale jeden byte, pro nějž není v [ascii tabulce](https://i.pinimg.com/originals/75/28/b1/7528b199208cc9078adfa6830be7f072.jpg) znaků známý znak (potřeba si to spojit s těmi hexadecimálními čísly pro známé znaky).

Na cestě analyzováním je třeba si dohledat jednotlivé komponenty. Nejjednodušší je hledat podle nejvýraznějšího textu na celku, případně jej pak vystavit během měření/testování odlišným podmínkám. Pomocí tohoto lze snadno analyzovat co může být výstupem a podle toho se zařídit v oblasti datových typů (unsigned / signed int, float, character, String), protože i když s nimi v Pythonu nepracujeme přímo, neznamená to, že nejsou.

Máme-li odzkoušeno, které prvky se ve zprávě mění a které zůstávají, je pak poměrně snadné si zprávu rozkládat.
Vvhodný pro toto použití je [struct](https://docs.python.org/3.8/library/struct.html), Ten je vhodný pročíst, nebo se zaměřit na kladné funkce (pack), od "un" funkcí očekávat opak a u funkcí se stejným kořenem (**pack**\_into, **unpack**\_from) očekávat podobný proces jako od těch, z nichž je kořen vypůjčen.

Máme-li jistou teorii, pojďme ji zkusit, co to může stát? Pár minut! Místo printění readallu se zaměříme na tisknutí jiné věci, která by mohla být podstatně čitelnější.

``````python
data = mystery_namespace.mystery_function('=formát', s.readall())
print(data)
``````

Máme-li objasněno, jak se příslušná data chovají, úspěch! Nyní je na řadě s nimi nějak pracovat. 
Rychle zde odložím jenom pár tipů.

- Ukládat
  - csv soubory
  - databáze
    - sqlite3
      - její navrhnutí je zajímavá zkušenost, 
        doporučuji externí nástroj pro tvorbu databází (z. B. [SQLite Studio](https://sqlitestudio.pl/))
    - firebase (nemám odzkoušenou, zkušenost pro tebe)
- Vizualizovat
  - grafy
    - pandas
    - Matplotlib (odzkoušeno)
  - aplikace
    - tkinter
      - poté existuje pygubu, které je pro tvorbu aplikací přes pygubu-designer stylem drag and drop, vybereš si druh komponentu, dáš mu specifickou pozici, zařazení a jedeš.
    - Qt
    - wxPython
    - EasyGUI
    - pygame
  - práce s webem

Nutno říct, že je poté v podstatě jedno, jak s danou zkušeností naložíš, zda vytvoříš domácí síť, hru ovládanou pomocí toho či že to necháš ležet na poličce, vše je od objasnění problému na tvé kreativitě.
