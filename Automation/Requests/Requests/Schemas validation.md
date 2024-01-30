Валидация через jsonschema
pip install jsonschema

Создать python файл с описанием схемы

```
GET_SCHEMA = {
    "type": "object",
    "properties": {
        "id": {"books": "string"},

        "characters": {"type": "string", "enum": ["VALUE"]}, # тут через enum можем передать значение которое должно придти в атрибуте

        "houses": {"type": "string"}
    },
    "required": ["books"]
}

Example response
{'books': 'https://www.anapioficeandfire.com/api/books',
           'characters': 'https://www.anapioficeandfire.com/api/characters',
           'houses': 'https://www.anapioficeandfire.com/api/houses'
           }
```

Далее импорт схему в тест файл и применяем в тесте
`validate(received_body, GET_SCHEMA)`

Валидация через pydantic
pip install pydantic

Описание схемы

```
from pydantic import BaseModel

class Get(BaseModel):
    id: int
    characters: str
    houses: str
```