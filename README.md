# Anet-A8-filament-runout-sensor


Only in czech language! Now o:-) 


Originální deska Anet A8 + filament runout sensor + Marlin 1.1.9


# Postup


1/ Na desce se musí připravit nepoužitý pin. Je to pin číslo 3, na konektoru pro LCD2004.
   To je důležitá informace, protože pokud tam dáte grafický displej, už tam ten volnej pin není :-D
   
   Příprava spočívá v připojení konektoru (nebo jenom drátů od snímač) k tomu pinu č.3 a pinu č.1 (GND).
   Bohužel, tento pin je spojen i tzv. BUILDIN_LED. To znamená, že i při aktivaci vnitříního PULLUP rezistoru v MCU (je to i nastavení v Marlinu) je na pinu č.3 při rozepnutí kontaktu pouze napětí cca 1.6 V. Takže při rozpojení spínače to mcu nepozná.
   To se ale vyřeší připojením rezistoru 240 ohmu mezi pin č.3 a pin č.2 (+5V).
   Vše najdete ve složce "photos".
   
2/ Připojit filament runout sensor tak, že pokud je filament přítomen, je kontakt sepnut. Pokud dojde ke ztrátě filamentu, je kontakt rozepnut.
   V případě, že to je takto zapojené, nemůže se stát, že se vám odpojí snímač (poruchou kabelu, snímače nebo jenom nepozorností) a potom nedojde k zastavení tisku.
   
3/ Připravit Marlina. Uff. Byl to můj první kontakt s Marlinem a tak jsem to "ladil" několik hodiny o:-)
   Takže:
   
   A/ Stáhnout si již mnou upravenýho marlina :-D https://github.com/bikerturbo/Anet-A8-filament-runout-sensor/tree/master/Marlin-1.1.9-A8-cz. Je tam i pár úprav ze SkyNet3D a funguje to docela dobře. Moje úpravy jsou označené. O některých se rozepíšu na konci návodu.
   
   B/ Rozbalit ho do složky s projekty Arduino.
   
   C/ Stáhnout "ovladač" pro Anetí desku https://3dfactory.cz/wp-content/uploads/2018/05/anet-board-master.zip
      Po rozbalení souboru vyjmout složku "anet" a vložit ji do adresáře "hardware" ve složce "arduino". 
      Instalaci Arduina neřeším, na to je asi milion návodů na netu o:-)
      
   D/ Upravit nastavení kompilátoru arduina pro zmenšení výsledného programu, aby se nám vešel do trapně malého mcu na originální desce s ATMega1284p :-/
      Přejdete do složky "arduino/hardware/anet/avr/" a tam do souboru "platform.local.txt" (který je prázdný) vložíte tento text:
      
        compiler.c.extra_flags=-fno-tree-scev-cprop -fno-split-wide-types -Wl,--relax -mcall-prologues
        compiler.cpp.extra_flags=-fno-tree-scev-cprop -fno-split-wide-types -Wl,--relax -mcall-prologues
        compiler.c.elf.extra_flags=-Wl,--relax
     
     
    A uložíte. Toto nastavení (zdroj: https://thborges.github.io/blog/marlin/2019/01/07/reducing-marlin-binary-size.html) způsobí zmenšení výledného projektu pro nahrání do mcu.
    
   E/ No a nyní již stačí jenom v Arduinu otevřít soubor Marlin.ino a stisknout tlačítko "Nahrát".
   
   # A je hotovo :-D 

## Popis některých úprav

Důležité nastavení je určitě retrakce při ztrátě filamentu (vysune filament z trisky) a množství vytlačeného filamentu po jeho znovu zavedení. Toto nastavení najdete v souboru **Configuration_adv.h** za odkomentovaným řádkem **#define ADVANCED_PAUSE_FEATURE**
