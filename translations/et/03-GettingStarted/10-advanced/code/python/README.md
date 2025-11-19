<!--
CO_OP_TRANSLATOR_METADATA:
{
  "original_hash": "c3c28b090a54f59374677200e23a809e",
  "translation_date": "2025-10-11T11:52:13+00:00",
  "source_file": "03-GettingStarted/10-advanced/code/python/README.md",
  "language_code": "et"
}
-->
# Käivita näidis

## Loo virtuaalne keskkond

```sh
python -m venv venv
source ./venv/bin/activate
```

## Paigalda sõltuvused

```sh
pip install "mcp[cli]"
```

## Käivita kood

```sh
python client.py
```

Peaksid nägema järgmist teksti:

```text
Available tools: ['add']
Result of add tool: meta=None content=[TextContent(type='text', text='8.0', annotations=None, meta=None)] structuredContent=None isError=False
```

---

**Lahtiütlus**:  
See dokument on tõlgitud AI tõlketeenuse [Co-op Translator](https://github.com/Azure/co-op-translator) abil. Kuigi püüame tagada täpsust, palume arvestada, et automaatsed tõlked võivad sisaldada vigu või ebatäpsusi. Algne dokument selle algses keeles tuleks pidada autoriteetseks allikaks. Olulise teabe puhul soovitame kasutada professionaalset inimtõlget. Me ei vastuta selle tõlke kasutamisest tulenevate arusaamatuste või valesti tõlgenduste eest.