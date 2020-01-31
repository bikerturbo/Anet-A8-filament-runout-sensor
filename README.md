# Anet-A8-filament-runout-sensor


Only in czech language! Now o:-) 


Originální deska Anet A8 + filament runout sensor + Marlin 1.1.9


# Postup


1/ Na desce se musí připravit nepoužitý pin. To je pin číslo 3, na konektoru pro LCD2004.
   To je důležitá informace, protože pokud tam dáte grafický displej, už tam ten volnej pin není :-D
   
   Příprava spočívá v připojení konektoru (nebo jenom drátů od snímač) k tomu pinu č.3 a pinu č.1 (GND).
   Bohužel, tento pin je spojen s tzv. BUILDIN_LED, to znamená, že i při aktivaci vnitříního PULLUP rezistoru v MCU (je to i nastavení v Marlinu) je na pinu napětí cca 1.6 V, což znamená, že nedojde k aktivaci připojeného snímače.
   To se ale vyřeší připojením rezistoru 240 ohmu mezi pin č.3 a pin č.2 (+5V).
2/ Připojit filament runout sensor, v tomto případě tak, že pokud je filament přítomen, je kontakt sepnut. Pokud dojde k rozepnutí, je kontakt rozepnut.
   V případě, že to je takto zapojené, nemůže se stát, že se vám odpojí snímač a nedojde k zastavení tisku.
3/ Připravit Marlina. Uff. Byl to můj první kontakt s Marlinem a tak jsem to "ladil" asi čtyři hodiny o:-)
   Takže:
   A/ Stáhnout marlina :-D https://github.com/MarlinFirmware/Marlin, vybrat verzi 1.1.x a stáhnout.
   B/ Rozbalit podle návodu do složky s projekty Arduino.
   C/ Stáhnout si "ovladač" pro Anetí desku https://3dfactory.cz/wp-content/uploads/2018/05/anet-board-master.zip
      Ze souboru vyjmout složku "anet" a vložit ji do adresáře "hardware" ve složce "arduino". 
      Instalaci Arduina neřeším, na to je asi milion návodů na netu o:-)
   D/ Upravit nastavení kompilátoru arduina pro zmenšení výsledného programu, aby se nám vešel do trapně malého mcu na originální desce s ATMega1284p :-/
      Přejdete do složky "arduino/hardware/anet/avr/" a tam do souboru "platform.local.txt" (který je prázdný) vložíte tento text:
      
        compiler.c.extra_flags=-fno-tree-scev-cprop -fno-split-wide-types -Wl,--relax -mcall-prologues
        compiler.cpp.extra_flags=-fno-tree-scev-cprop -fno-split-wide-types -Wl,--relax -mcall-prologues
        compiler.c.elf.extra_flags=-Wl,--relax
     
     
   A uložíte. Toto nastavení (zdroj: https://thborges.github.io/blog/marlin/2019/01/07/reducing-marlin-binary-size.html) způsobí zmenšení výledného projektu pro nahrání do mcu.
   E/ Upravíme nastavení Marlina. Bude to strohé. Nejprve uvedu název souboru a poté co se má jak změnit :-D
      
      Configuration.h -> 
      
      Kašlu na to. Rovnou si stáhněte Marlina, co mám tady nahranýho o:-) 
      Je tam i pár úprav ze SkyNet3D a funguje to docela dobře....
