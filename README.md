# PerfectAgentTeam

PerfectAgentTeam, kurumsal standartlarda cok-agentli yazilim gelistirme akisini standardize etmek icin tasarlanmis bir agent konfigurasyon reposudur.

## Nedir?

Bu repo, farkli sorumluluk alanlarina ayrilmis agent tanimlari ve merkezi yonetim kurallari sunar:

- Uzmanlik bazli agent rolleri
- Deterministic karar zinciri
- Guvenlik, kalite ve tutarlilik gate'leri
- Uretim ortami odakli (production-ready) calisma modeli

## Icerik

- `.github/agents/`: Rol bazli agent tanimlari
- `.github/copilot-instructions.md`: Tum sistem icin birlesik davranis, kalite ve guvenlik kurallari

## Tasarim Ilkeleri

- Guvenlik once gelir
- State tutarliligi zorunludur
- Test ve dogrulama olmadan merge yapilmaz
- Belirsiz kararlar yerine acik, izlenebilir karar kaydi tutulur

## Kullanim

1. Repoyu klonlayin.
2. Agent tanimlarini ihtiyaca gore ozellestirin.
3. `copilot-instructions.md` icindeki zorunlu gate ve workflow kurallarini takip edin.

## Hedef

Amac, tek bir agente bagimli olmayan; denetlenebilir, guvenli ve surdurulebilir bir gelistirme sistemi kurmaktir.
