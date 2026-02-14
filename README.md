# Ilana-works
It is a homework
from typing import List, Optional
from src.animal import Animal


class Zoo:
    """
    Класс Zoo, управляющий коллекцией животных.
    """

    def _init_(self, name: str, location: str) -> None:
        """
        Инициализация зоопарка.
        
        Аргументы:
            name: Название зоопарка
            location: Локация зоопарка
        """
        self._name = name
        self._location = location
        self._animals: List[Animal] = []



    
