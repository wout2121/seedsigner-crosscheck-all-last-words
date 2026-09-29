# SeedSigner Crosscheck – Alle mogelijke laatste woorden

Een offline HTML-tool die **alle geldige mogelijkheden** voor het laatste woord (woord 12 of 24) van een BIP39-seed phrase toont. Zo kun je controleren of het laatste woord dat je SeedSigner (of een andere wallet) voorstelt, echt een geldig woord is.

## Waarom deze tool?

Ik heb de code van @M21 geforkt en aangepast zodat je de SeedSigner-software **gemakkelijker kunt crosschecken**.

> **Cruciaal:** download de SeedSigner-software op een **ander apparaat** dan het apparaat dat je voor deze crosscheck gebruikt.

**"Waarom is dit nog nodig als de GPG-keys al geverifieerd zijn?"**
Een geldige GPG-handtekening bewijst alleen dat het bestand dat je downloadde echt van de ontwikkelaars komt. Ze beschermt je niet tegen het apparaat waarop je dat doet. Staat er malware op je computer, dan kan die nog steeds heel wat manipuleren, zoals de **12 (of 24) woorden** van je seed phrase, de **private keys**, de **xpubs** of de **ontvangstadressen**.

Door op een **tweede, onafhankelijk apparaat** te controleren, moet een aanvaller beide apparaten tegelijk gecompromitteerd hebben om je te misleiden.

## Wat doet de tool?

Het laatste woord van een BIP39-seed bestaat deels uit vrije (willekeurige) bits en deels uit een **checksum**. Daardoor zijn er meerdere geldige laatste woorden mogelijk:

| Seedlengte | Vrije bits in laatste woord | Checksum-bits | Aantal geldige laatste woorden |
|---|---|---|---|
| 12 woorden | 7 | 4 | 128 |
| 24 woorden | 3 | 8 | 8 |

Je typt de eerste 11 (of 23) woorden en de tool toont **elk geldig laatste woord**, met het nummer uit de BIP39-woordenlijst. Staat het woord van je SeedSigner niet in de lijst, dan klopt er iets niet.

Daarnaast bevat de tool ook de hex-/dobbelsteen-entropie zoals SeedSigner (50 tekens → 12 woorden, 99 tekens → 24 woorden, via SHA-256). De tool stopt bij de woorden: geen adressen, geen QR-codes en geen private keys in het geheugen.

### Verschil met de andere tool

- **Deze tool:** toont **alle** geldige laatste woorden (128 of 8).
- [wout2121/seedsigner-crosscheck-last-word](https://github.com/wout2121/seedsigner-crosscheck-last-word): geeft **precies één** laatste woord, met de vrije bits vastgezet op 0.

## Bestand

| Bestand | Uitleg |
|---|---|
| `all-last-words-possibilities.html` | De tool zelf: één zelfstandig HTML-bestand, zonder netwerkverkeer en zonder opslag (strikte Content-Security-Policy). |
| `all-last-words-possibilities.html.sig` | De GPG-handtekening van het HTML-bestand. Beide bestanden horen samen. |

## Downloaden en controleren

1. Download **beide** bestanden (`.html` en `.html.sig`) via [Releases](https://github.com/wout2121/seedsigner-crosscheck-all-last-words/releases/latest) (Source code zip), of klik op elk bestand en kies **Download raw file**.
2. Controleer de GPG-handtekening:

   ```bash
   gpg --verify all-last-words-possibilities.html.sig all-last-words-possibilities.html
   ```

   De handtekening is gemaakt met de sleutel met fingerprint:

   ```
   CE1F B111 C2B5 FECC 91C3  6F11 C464 F440 9ED9 FEF2
   ```

   Je moet deze publieke sleutel eerst importeren (`gpg --import`) en de fingerprint via een onafhankelijk kanaal controleren.

3. Optioneel: controleer de SHA-256-hash:

   ```bash
   sha256sum all-last-words-possibilities.html
   # bb0c05c68e07d9c5ead231dc659da04aab14a1f2979940d769e62873c6e1846c
   ```

## Veilig gebruiken

1. Download de SeedSigner-software op **apparaat A**.
2. Download deze tool op **apparaat B** (een ander apparaat).
3. Koppel apparaat B **los van het internet** voordat je het HTML-bestand opent.
4. Typ de eerste 11 (of 23) woorden en controleer of het laatste woord van je SeedSigner in de lijst staat.
5. Sluit het tabblad na gebruik en gebruik voor echte fondsen bij voorkeur een apparaat dat nooit meer online komt (of een live-USB).

## Meer tools

- [wout2121/seedsigner-crosscheck](https://github.com/wout2121/seedsigner-crosscheck): single-sig/multisig, fingerprints, xpubs en adressen
- [wout2121/seedsigner-crosscheck-last-word](https://github.com/wout2121/seedsigner-crosscheck-last-word): één laatste woord (0 vrije bits)
- [wout2121/stappenplan-gpg-verifieren](https://github.com/wout2121/stappenplan-gpg-verifieren): stappenplan om GPG-sleutels te verifiëren

## Disclaimer

Gebruik op eigen risico. Deze tool is bedoeld als extra controle, niet als vervanging van goede operationele beveiliging. Test altijd eerst met een testseed voordat je er echte fondsen aan toevertrouwt.
