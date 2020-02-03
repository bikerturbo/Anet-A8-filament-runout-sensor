# Anet-A8-filament-runout-sensor


Only in czech language! Now o:-) 

Návod je určený pro originální desku Anet A8 (bootloader jsem nemusel nahrávat) a originální displej LCD2004.
Po dokončení budete mít ve vaší Anetce Marlin 1.1.9 a funkční filament runout sensor.


## Popis úpravy


1. Na desce se musí připravit nepoužitý pin. Je to pin číslo 3, na konektoru pro LCD2004.
   Pokud plánujete použít grafický displej, už tam ten volnej pin nebude :-D
   
   Příprava spočívá v připojení konektoru (nebo jenom drátů od snímač) ke zmíněnému pinu č.3 a pinu č.1 (GND).
   Jelikož je ale k pinu č.3 připojena i tzv. BUILDIN_LED, je na pinu v klidovém stavu (pin není spojen s GND) napětí pouze cca 1.6 V a ne požadovaných 5 V.
   Tento problém se hravě vyřeší připojením rezistoru 240 Ohmu mezi pin č.3 a pin č.2 (+5V).
   
2. Připojit filament runout sensor tak, že pokud je filament přítomen, je kontakt snímače sepnut. Pokud dojde ke ztrátě filamentu, je kontakt snímače rozepnut.
   Toto zapojení eliminuje poruchu snímače. Tzn, že pokud je snímač odpojen vlivem ulomeného vodiče nebo nepozorností obsluhy, tiskárna to považuje za strátu filamentu a dojde k zastavení tisku ;-)
   
3. Připravte si Marlina, Arduino a ovldače:
   
   - Ze stránek [Arduino.cc](https://www.arduino.cc/en/Main/Software) si stáhněte a nainstalujte si Arduino IDE.
   
   - Po dokončení instalace arduina si stáhněte Marlina https://github.com/MarlinFirmware/Marlin/archive/1.1.x.zip.
   
   - Stažený soubor rozbalte do vaší složky s projekty Arduino.
   
   - [Zde](https://3dfactory.cz/wp-content/uploads/2018/05/anet-board-master.zip) si stáhněte "ovladač" pro Anetí desku.
     Po stažení a rozbalení souboru, zkopírujte složku "anet" do složky "/arduino/harware/".
     
   - Nyní upravte nastavení kompilátoru arduina pro zmenšení výsledného programu, aby se nám vešel do paměti mcu na originální desce s ATMega1284p. Přejděte do složky "/arduino/hardware/anet/avr/" do souboru "platform.local.txt" (který je zatím prázdný) vložte a uložte tento text:
   
            compiler.c.extra_flags=-fno-tree-scev-cprop -fno-split-wide-types -Wl,--relax -mcall-prologues
            compiler.cpp.extra_flags=-fno-tree-scev-cprop -fno-split-wide-types -Wl,--relax -mcall-prologues
            compiler.c.elf.extra_flags=-Wl,--relax
   
     (zdroj: https://thborges.github.io/blog/marlin/2019/01/07/reducing-marlin-binary-size.html).
     
 4. Spustíte arduino a "Nástrojích" vyberete položku "Vývojová deska:" a vyberete "Anet V1.0".
    Poté otevřete soubor "Marlin.ino" ve složce "Marlin" a stisknete tlačítko "Ověřit".
    
    Kompilace trvá kapku déle. Pokud je vše OK, vypíše vám arduino podobné hlášení:
    
               Projekt zabírá 112814 bytů (88%)  úložného místa pro program. Maximum je 126976 bytů.
               Globalní proměnné využívají 4169 bytů dynamické paměti.
               
    V tuto chvíli připojte USB kabel k desce Anet (tiskárna musí být puštěná).
    
    Zkotrolujte si že se vám deska objevila v položce "Port" (v "Nástrojích") jako další com port (win) nebo ttyUsb (linux).   
   
    Nyní můžete stisknout tlačítko "Nahrát".
    
    ###### A to je vše. Nyní máte ve vaší Anetce Marlina a filament runout sensor.
    
 
